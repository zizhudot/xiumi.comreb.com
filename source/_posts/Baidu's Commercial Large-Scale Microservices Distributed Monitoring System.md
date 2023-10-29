---
title: Baidu's Commercial Large-Scale Microservices Distributed Monitoring System -- Fengmu
description: Baidu's Commercial Large-Scale Microservices Distributed Monitoring System -- Fengmu
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---


**Introduction**: As an early access party and a core member of Phoenix Eye, I have experienced the changes of the whole project for four years before and after, and have seen the difficult beginning of the project, the silent accumulation in the middle, and the vigorous development in the late stage. Every architectural change carries the imprint of the technology wave, and I have also seen project members utilize limited resources to solve practical problems and continue to innovate.



Phoenix Eye is the performance monitoring system (APM) for Baidu's commercial business system, which focuses on monitoring Java applications and basically accesses most of Baidu's Java applications (covering thousands of business applications and tens of thousands of containers). It is capable of automatically burying the mainstream middleware frameworks (Spring Web, RPC, database, cache, etc.), realizing full-stack performance monitoring and full-link tracking and diagnosis, and providing Baidu's various lines of business with microservices system performance indexes, business gold indexes, health status, and monitoring alerts.






**Phoenix product flowchart*.



+ **Data collection**: Phoenix Eye Probe technology can be automatically implanted into the business process to collect relevant performance information, and the business process is completely senseless.
    
+ **Data Calculation and Analysis**: According to the type, the timing data is stored in TSDB, the timing database of Baidu SIA Intelligent Monitoring Platform, which is used to generate visual reports and exception alarms. The call chain data will be stored in Palo (open source named Doris) big data warehouse for topology analysis and call chain retrieval.
    
+ **Application Scenarios**: As mentioned above, Phoenix Eye provides stability reports, exception alerts, error stack analysis, service time consumption analysis, call topology analysis, business log correlation analysis, etc.
    






*Timeline of Phoenix Contact's Architecture Changes



**01 Phoenix Eye Project



The project was initiated in 2016, and the middleware of Baidu Phoenix Nest advertising business system (distributed RPC framework Stargate, etc., configuration center, database middleware, etc.) had been perfected. With the deepening of the monolithic service split, the overall Java online deployment scale gradually becomes more and more, and at the same time, more and more problems are exposed.



Typical problems are:



1. Long cycle of core service problem localization. It took a long time to locate the problem after a large number of errors were reported by multiple modules.
    
2. The cost of obtaining cluster logs is very high, and the lack of log call chaining leads to a high cost of localization, and even some problems cannot be localized.
    
3. Exception logs need to be logged into specific online instances to view them. The number of online deployment instances is large, and the troubleshooting time is long.
    



Phoenix Nest business side urgently needs a distributed tracking system to complete the whole business side log "big chain". Therefore, Baidu Business Platform Infrastructure Group initiated the Phoenix Eye project, named "Phoenix Eye".






**02 Phoenix Eye 1.0



In the field of distributed link tracking, there are mainly intrusive and non-intrusive probes in the link collection. 1.0 probes take the intrusive way. The business developer first introduces probe-related dependency jar packages, and automatically collects call relationships as well as performance data through interceptors; then, adds hard-coded supplementary business data.






*Example of ∆coding*



The data collected by the probe is printed to disk and collected away via kafka. The underlying data processing and data storage uses Storm, Hbase and other popular data processing systems at the time. The back-end architecture is more complex.






* △ Phoenix Eye 1.0 architecture schematic diagram *



**03 Phoenix Contact 2.0



Phoenix Eye 2.0 version, the first is to reduce the probe access costs. 2.0 version, the probe to use java agent technology combined with cglib to do AOP annotations, the introduction of dependencies on the jar package from N to 1. From writing large parts of the call chain to the invocation of the call chain to the invocation of the invocation of the invocation of the invocation of the invocation of the invocation of the call chain. In version 2.0, probes use java agent technology combined with cglib to make AOP annotations, reducing the number of dependencies from N to 1, and changing from writing large parts of the call chain to fill the code to AOP as much as possible. the transport layer of the probe side uses a more efficient transport protocol (protobuffer+gzip), which is sent directly to kafka via the HTTP protocol to greatly reduce the disk IO overhead.






2.0 probes are easier to access and faster than 1.0. But still need to add business-side AOP code. For hundreds of applications on the business side, access is still a big project, and promotion is still difficult.



**04 PhoenixMe3.0**



During the design of Phoenix Eye 3.0 architecture, the project team members have been thinking about two issues:



1. how to let the business side access quickly, with as little change as possible, or even "no-perception access"?
    
2. how to reduce the difficulty of architecture operation and maintenance, not only can handle massive data, but also low-cost operation and maintenance?
    



In order to solve problem 1, Probe 3.0 decided to give up the intrusive approach completely, and replaced it with non-intrusive, i.e., bytecode-enhanced approach.



Several popular monitoring and diagnostic tools at the time were investigated:






*△Newrelic, pinpoint, greys monitor probe research



3.0 probe reference Greys support runtime enhancement features and pinpoint, Newrelic based on the plug-in extension development design concept. The final effect is that the probe can be automatically implanted in the business process monitoring code, monitoring the specific work to be completed by the plug-in system , fully oriented cut-side monitoring.






△ probe active loading schematic



The back-end storage system relies on Doris, Baidu's self-developed interactive SQL data warehouse based on MPP, which is compatible with the mysql protocol and has low learning costs. It can do both storage and analytical calculations, initially avoiding the introduction of spark, storm and other technologies to reduce the complexity of the system.






*△ Architecture design as shown in the figure*.



After the upgrade of the architecture, as a small team, it is also possible to quickly deploy probes in bulk, and the computational storage capacity can also meet the demand. As of 2017, Phoenix Eye 3.0 went live with more than 100 applications running on top of more than 1,000 containers.



***05 Phoenix Eye 4.0**



In 2018, a wave of microservices and virtualization swept in. With the continuous upgrading of the deployment platform and the maturity and perfection of the springboot system, the monolith can be quickly split into a large number of microservices, relying on the platform for efficient operation and maintenance deployment. Phoenix Eye was integrated by the microservice hosting platform as a basic component, and was popularized and applied at the company level, and the overall scale of deployed applications surged from hundreds to thousands, and the deployed containers changed from thousands to tens of thousands.






At this stage, a lot of problems erupted, and the core technical problems were mainly two:



1. Probe upgrades need to restart the business application to take effect, and the online application restart traffic is loss. This makes it difficult to frequently upgrade the probe version and quickly introduce new features.
    
2. 15 billion entries are written in real time every day, and the peak traffic is 300w entries/s. Data import is easy to be lost; retrieving a single call chain performance check takes about 100+ seconds.
    



In 2019, Phoenix Eye carried out further renovation and upgrading, targeting 1 and 2 problems, and carried out technical attacks.



The probe level studies how to support hot-plugging, that is, the probe automatically completes the upgrade while the business process is running. At first, in order to ensure the visibility of the business class to the probe plug-in class, the probe class was unified into the System Classloader. But System Classloader as the system default, does not support uninstallation. On the contrary, if you put all the probe classes into a custom class loader. The probe classes are completely invisible to the business classes, and no bytecode enhancement can be done.






*△Probe hot-pluggable classloader system *△Probe hot-pluggable classloader system



In order to solve the visibility problem, probe introduces the bridge class, through the bridge class provides the code stakes and plug-in class library projection, the user class can access the actual use of the probe class, to complete the purpose of monitoring and transformation. For different plug-ins, they are placed inside different custom Classloader. In this way, plug-ins are not visible to each other. A single plug-in can also be accomplished hot-plugging. Specific design details will be explained in detail later in the article.



Undoubtedly, Phoenix Eye Probe is the only hot-pluggable monitoring probe technology in the industry, and we have also applied for a patent. Its functional correctness and performance have been verified by large-scale online traffic.



Continue to push forward to optimize the performance of call chain retrieval.



First analyze our underlying storage structure:






Through the analysis of the slow query, it is found that the slow retrieval is mainly due to two reasons: first, a large number of queries do not go to any index, and the full table scanning of massive data is very slow. Secondly, too many import fragments, resulting in file Compaction is particularly slow, typical of LSM-Tree read amplification. In order to solve these problems, the call chain storage layer to reconstruct the table structure, through a large number of Rollup with the basic table, to optimize the query time.Doris at this time already have the ability to streaming import, but also take the opportunity to switch from small batch import to streaming import.






*Call Chain Processing Architecture






*The above figure shows the topology of the microservice panorama built by Phoenix Eye in real time. As of January 2020, it probably covers the online traffic topology of dozens of product lines, and the most fine-grained nodes are interfaces, i.e. functions in Java applications. From the figure, we can analyze that there are roughly 50w+ non-islanded interface nodes hosting the whole platform, and 200w+ interface node connections. *The following are some examples of the types of nodes that can be used in a Java application



**06 Data Processing Architecture Separation



The architecture continues to evolve, the amount of data collected by Phoenix Eye is increasing, and the demand from the business side is also increasing.



There are two main problems faced:



1. data visualization capability depends on front-end development, and a large number of multi-dimensional visualization and analysis needs are difficult to meet.
    
2. The call chain is sampled, resulting in inaccurate statistical data, which cannot meet the needs of statistical reports.
    



These two problems boil down to how time-series data are stored and presented. This involves two very basic concepts in the field of distributed tracing, timing and call chain data. The so-called time-series data is a series of time-based data used to view some metrics data and metrics trends. Call chain data is a record of the entire flow of a request, which is used to see where the request failed and where the bottleneck in the system is.



Time series data does not need to save details, only time, dimension and indicator data points, which can be stored in a specialized Time Series Database (Time Series Database). In the actual scenario, Phoenix Eye does not specifically maintain a time series database. Instead, it connects to TSDB, the distributed time series database of Baidu SIA Intelligent Monitoring Platform, and at the same time, it uses Baidu SIA platform to provide rich multi-dimensional visual analysis reports, which can be used to solve the user's needs of various visual multi-dimensional data analysis.






*‍△Current overall architecture *



**07 Conclusion



The whole project of Phoenix Eye has lasted for 4 years, and has experienced countless difficulties and bumps in the middle, and finally achieved milestone results by accumulating the continuous efforts of the project members. This article briefly introduces the business background, technical architecture and product form of Phoenix Eye, and will continue to introduce the technical details of the realization of the article, welcome to continue to pay attention.