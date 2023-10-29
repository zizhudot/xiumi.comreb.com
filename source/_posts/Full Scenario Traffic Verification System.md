---
title: Full Scenario Traffic Verification System
description: Full Scenario Traffic Verification System
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

This paper introduces a practical solution to verify the functionality and performance of a reconfigured system based on online traffic. Detailed descriptions of how to intercept, record, store, playback, and pressurize online traffic are provided to provide a reference idea for readers with similar needs.

## 1 Business Background

With the start of the BCinks project, the middle office needs to close the order flow and switch all the order taking entrances of ECLP and BP to the BCinks unified order taking system. And the invocation of the various order taking entrances is different, there are JOS requests (external merchants), JSF requests (such as TC), there are also MQ asynchronous messages (such as POP). In order to ensure that each system smoothly cut the volume and minimize the risk of cutting the volume, it is necessary to do sufficient traffic verification (including functional verification and performance verification) before cutting the volume. To this end, a full-scenario traffic verification system is designed to support AB verification (functional verification) and pressure testing (performance verification) based on online traffic, which provides a reliable basic support for each business line to take orders and cut the volume.

## 2 Explanation of Terms

+ Flow diversion: Introducing the online traffic of the system where each order taking portal is located to the flow verification system.
+ Record: Copy the online traffic and make persistent storage.
+ Playback: Play the recorded traffic to the system to be verified.
+ Cutting: Switch the order taking traffic from the old order taking system such as ECLP to the new BCinks unified order taking system.
+ AB Verification: The online traffic hits the formal environment and AB environment at the same time, and the results of the two environments are compared and analyzed to verify the correctness of the AB environment.

## 3 Design Ideas

** How to attract traffic? **  
Traffic diversion can be realized by introducing traffic agents in the business system.

** How to record? **  
Considering the need to support large data volume as well as composite queries, ES is chosen as the persistent storage solution.

** How to playback? **  
In order to avoid dependence on Jar packages of various business systems, we choose to use JSF generalized calls to achieve traffic playback.

** Is there a similar system available? **  
Moonlight Box (jcase): a traffic recording and playback system developed by Jingdong Retail. It supports traffic recording and playback functions, but it does not meet some personalized needs, such as recording according to custom business rules, cut volume control, and so on.

## 4 System Design

## 4.1 Overall design

Traffic agent: diverts traffic to the verification system through interception, filtering and reporting.  
Recording Service: Receive the online traffic introduced by the traffic proxy and make persistent storage.  
Playback engine: uses the recorded online traffic to request the target interface to be verified.  
Pressure testing engine: uses the recorded online traffic to send multi-threaded pressure to the target interface to be verified.


### 4.2 Detailed Design

#### 4.2.1 Traffic Proxy

1) Generic traffic proxy


Introduce traffic proxy in the business system, intercept (JSF Filter or AOP) online traffic through the traffic proxy, and report the traffic to the recording service through asynchronous MQ for persistent storage.

2) JOS Traffic Proxy


External merchants call the JOS platform via HTTP, and the JOS platform internally transfers to JSF to call the order taking service. In order to make external merchants senseless, publish a JSF service (virtual service) with exactly the same interface as the business system, the difference is to provide a new alias, switch to the new alias through the JOS platform configuration, so that the JOS traffic is introduced to the recording agent, and then the recording agent reports the traffic to the recording service for persistent storage through the asynchronous MQ method.

#### 4.2.2 Traffic Storage

Recorded traffic is persistently stored to ES, and recording tasks are created in accordance with the \[interface:method\] dimension, and the primary keys of records under the same recording task are all prefixed with the recording task number, followed by a numeric increment, and the maximum suffix (cached in Redis) is the total number of records recorded under that recording task.

| **Attribute name** | **Example value** | **Example value** |
| --- | --- | --- |
| id | RT7625109167934456\_1 | Primary key identifier |
| recordData | {"args":\[{"fakeNo": "fakeNo001"}\], "argsType":\["cn.jdl.baichuan.router.replay.contract.domain.fake.FlowFakeRequest"\]] , "attachments":{"traceId": "8112206384546625", "type": "1"}, "clazzName": "cn.jdl.baichuan.router.replay.contract.service. RouterFlowFakeService", "methodName": "match", "resultObj":true} | Recorded body |
| recordTaskNo | RT7625109167934456 | Recorded task number |
| timestamp | 1636719778929 | time stamp |

#### 4.2.3 Traffic Playback

Support single, batch, and batch playback by recording task dimension. The playback call adopts JSF generalized call mode, avoiding the dependence on the Jar package of the business system.

At the same time of traffic playback, it supports the configuration of comparison services, comparing the service reception input parameters and the output results of the old and new interfaces, and it can compare and analyze the processing results of the old and new interfaces, so as to verify the correctness of the new interface functions.

#### 4.2.4 Traffic Pressure Measurement

In order to realize the effect of sending pressure, it is necessary to use multi-machine and multi-thread concurrently to request the target interface. However, multiple machines and threads share the same recorded data as the pressure data source. Therefore, before actually sending pressure, it is necessary to assign the data for each execution thread, and each thread only takes its own data without interfering with each other.

Pressure generation strategy (master-slave architecture, master allocation, slave execution)


Pressure test engine adopts master-slave architecture, the press is divided into master and slave nodes, the master node is responsible for receiving pressure test requests and assigning pressure test tasks; the slave node is responsible for executing pressure test tasks.

Data allocation strategy (average by volume, residual polling, sliding window)


1) Calculation Window

According to the total amount of recordings in the recording task, it is evenly distributed to each thread, and the remainder is then distributed to each thread by polling until it is finished, so that the number of recordings allocated to each thread can be determined (the size of the window);

2) Slide by window

Tile all the recording tasks horizontally from left to right, each thread according to its own window size from left to right in order to occupy the recording records.

## 5 Business Practice

## 5.1 Cut Volume Verification

Take the switching of POP order taking interface for warehousing and distribution as an example, we need to replace the original ECLP-SO system with a new order center. Before the switchover, the ECLP-SO system will still provide online order taking service, but at the same time, it will record the online traffic through the traffic verification system and put it back to the new order center. The order taking function of the new order center will be verified by comparing the results of the old and new systems on the same order taking request. Only after sufficient functional verification will the order taking traffic be switched to the new order center, thus greatly reducing the risk of volume cutting.


### 5.2 Requirement Iteration

The product verification service is a core interface provided by the product center to the outside world, and the interface logic is complex, so every demand iteration is a great challenge to go online. Even after verification in the test environment and pre-release environment, there is still no 100% guarantee that there will be no impact on the online business after going live. After all, the test environment, pre-release environment verification request parameters are single and limited, and cannot reflect the diversity and complexity of online requests. Therefore, the product center accessed the traffic verification system, each time there is a new demand iteration before going online, the first recording of online traffic, using the real online traffic in the pre-release environment for full verification before doing online operations. This greatly reduces the chance of damage to the online business due to inadequate verification, provides a layer of security for the online business, and improves the stability of the online system.
