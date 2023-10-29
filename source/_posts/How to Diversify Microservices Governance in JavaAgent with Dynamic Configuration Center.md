---
title: How to Diversify Microservices Governance in JavaAgent with Dynamic Configuration Center
description: How to Diversify Microservices Governance in JavaAgent with Dynamic Configuration Center
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

## I. Preface

With the wide application and development of JavaAgent in microservice governance, we can monitor, manage and adjust microservices at runtime to meet different business requirements and operating environments. However, as the complexity of microservice architectures increases, it becomes more and more difficult to manage and configure the governance of microservices, so it becomes crucial to implement microservice diversity governance in JavaAgent using dynamic configuration centers.

Sermant is an agentless service grid based on Java bytecode enhancement technology that supports diverse governance of microservices through dynamic configuration. The following is the microservice architecture of Sermant:


Sermant does not directly provide a dynamic configuration center , but Sermant based on different configuration centers to achieve the dynamic configuration function , based on the function Sermant can not only listen to the mainstream configuration center configuration information modification , but also for different scenarios for configuration listening , for example : Sermant can not only listen to the service configuration changes , but also can listen to the application of the global configuration changes. Based on this feature can better help developers and operation and maintenance personnel to manage the ability of microservice governance.

## Sermant's Dynamic Configuration Model

Sermant Dynamic Configuration Model is a configuration management solution based on a hierarchical model, and its core components include Group and Key. Sermant isolates configuration items through different Groups (grouping information) to make configuration management more flexible and scalable; at the same time, it identifies specific attributes of the configuration items through the Key to realize precise control and efficient maintenance of configuration items. At the same time, the identification of specific attributes of configuration items by Key realizes precise control and efficient maintenance of configuration items.

In the Sermant dynamic configuration model, the number of Groups should not be too large; the implementation of Groups in the Sermant dynamic configuration model is based on the data model of the configuration center, for example, the namespace of Nacos, which is used to realize the isolation of the tenants, and should not be too large, so the number of Groups in the Sermant dynamic configuration model should not be too large. The number of Groups for the Sermant dynamic configuration model should also not be too large.

In contrast to Groups, the Sermant dynamic configuration model allows the creation of multiple Keys, but a single instance should not subscribe to too many Keys. This is because in the process of subscription and maintenance of configuration items, if a single instance subscribes to too many Keys, it may lead to performance problems of the service, and even cause problems of untimely configuration updates or configuration conflicts. Therefore, controlling the number of Keys subscribed by a single instance is one of the key factors to ensure the efficiency and availability of configuration management.

Sermant Dynamic Configuration Model realizes comprehensive coverage and efficient support for complex configuration scenarios through the organic combination of Groups and Keys. At the same time, by controlling the number of Groups and Keys, it simplifies the subscription and update process of configuration items and improves system availability and maintainability.

Sermant's dynamic configuration model is shown below:


+ ** Configuration model implementation based on Zookeeper** ** **

Zookeeper uses a tree-like data model that will store data information on a single data node, these nodes are called Znode, Znode is the smallest data unit in Zookeeper, a Znode can have multiple child nodes. A unique Znode that can be determined by a path such as /zookeeper/key1. as shown in the following figure:


Sermant's dynamic configuration model is based on the Zookeeper Configuration Center implementation as shown below:

Group (grouping information): Parent node path information of the Znode node as Group information

Key (configuration item name): the node name of the Znode node is used as the Key of the configuration item, and the node data is used as the specific configuration content.

Znode nodes can be isolated through different paths, and nodes with the same node name can exist under different paths. Guaranteed Sermant isolates configuration items through different Groups (grouping information), and the same Key (configuration item name) can exist under the same Group (grouping information).

+ **Configuration model implementation based on Nacos ******

Nacos data model is a hierarchical model, for Nacos configuration management, through Namespace, Group, Date ID can locate a configuration set. As shown in the following figure:


Namespace (Namespace) is mainly used for configuration isolation in different environments**, **Group (configuration grouping) is mainly used for different projects or applications**, **Configuration set can contain a variety of configuration information for the system, each configuration set ID that is Data Id. one configuration content contained in the configuration set is a configuration item. It represents a specific configurable parameter and its value field, usually in the form of key=value.

Sermant's dynamic configuration model is based on the Nacos Configuration Center implemented as follows:

Group: Nacos Namespace and Groups are combined as group information for Sermant's dynamic configuration model.

Key (configuration item name): Nacos configuration set ID (Data Id) as the configuration item name of Sermant's dynamic configuration model.

Configuration set (Data Id) can be segregated by different Namespace (Namespace) and configuration group (Group), different Namespace (Namespace) and configuration group (Group) can exist under the same name configuration set (Data Id). Ensure that Sermant segregates configuration items by different Groups, and that the same Key (configuration item name) can exist under the same Group.

+ **Configuration model implementation based on ServiceComb Kie** **

ServiceComb-Kie (ServiceComb Key-Value Store) data model is based on Key-Value pairs to store and manage configuration information.ServiceComb-Kie controls the scope of effect of a configuration based on tags, and unique key-value pairs can be identified through tags and Keys.

Sermant's dynamic configuration model based on ServiceComb-Kie is implemented as follows:

Group (grouping information): ServiceComb-Kie's label information is used as the Group information of Sermant's dynamic configuration model.

Key (configuration item name): The configuration item name of ServiceComb-Kie is used as the Key of Sermant's dynamic configuration model.

Configuration items of ServiceComb-Kie can be segregated by different label information. Configurations with the same configuration item name can exist under different labels, and different configuration items can be configured under the same label information. It is guaranteed that Sermant isolates configuration items by different Group (grouping information) and the same Key (configuration item name) can exist under the same Group (grouping information).

## III. Best Practices for Sermant's Dynamic Configuration Model

Dynamic configuration is one of the core features of Sermant, which can help Sermant to unify the management of microservice governance capabilities, such as: gray scale release, flow limitation and degradation, link tracking, etc.. As shown in the figure below:


When using Sermant Dynamic Configuration for microservice service governance, avoid creating too many Group (grouping information) and Key (configuration item name) to reduce the complexity and confusion of the configuration, reduce the performance consumption caused by listening to too many configurations, and also improve the maintainability and readability of the configuration.

Next we explain the best practice of Sermant dynamic configuration through the labeled routing plugin.

## 1) Label Routing Plugin Features

Label Routing Plugin is the basis for Sermant to realize the microservice routing governance function. Label Routing Plugin configures the routing rules by service granularity or global granularity for service providers, divides the providers of a certain service or multiple services into the same grouping, and constrains the traffic to flow only in the specified grouping, so as to realize the purpose of traffic segregation. The label routing plug-in is also the capability base for scenarios such as traffic coloring, blue-green release, grayscale release, all-link grayscale, and same-availability-area priority invocation.

## 2) Dynamic Configuration Model of Label Routing Plugin

The label routing plug-in is based on Sermant's dynamic configuration model for rule configuration. In the microservice scenario, the dynamic configuration model of labeled routing plug-in is implemented as follows:

Group (grouping information): based on the application name appName and environment name environment composition, for example: app = $ { appName } & environment = $ { environment }

Key (configuration item name): for rules with service granularity, the key value is servicecomb.routeRule.${srviceName}, and ${ srviceName } is the microservice name of the target application. For rules targeting global granularity, the key value is servicecomb.globalRouteRule.

The configuration model implementation of the tagged routing plugin based on zookeeper configuration center is shown below:


Through the tag routing plugin based on the Sermant dynamic configuration model implementation we can see that the Sermant dynamic configuration model of the Group (grouping information) support based on the name of the application and the environment name generation, at this time for a single microservices application only need to create a Group (grouping information), you can avoid the creation of too many Group (grouping information). Sermant Dynamic Configuration Model Key (configuration item name) supports generation based on the microservice scenario and service name, at this time you can target a single service, you can also target all the services for dynamic configuration, to ensure that a single instance does not need to listen to multiple configurations, to avoid listening to multiple configurations leads to service performance degradation, configuration updates are not timely or configuration conflicts and other issues.

## Summary

Dynamic configuration in JavaAgent plays an important role in realizing the diverse governance of microservices, and is one of the important means of realizing microservice governance. Through dynamic configuration, the runtime state of microservices can be dynamically adjusted to realize the dynamic governance of microservices. For example, the load balancing policy of microservices can be adjusted through dynamic configuration according to the actual load situation to achieve more refined load balancing.

Using Sermant's dynamic configuration model for microservice governance not only realizes the dynamic governance of microservices, but also reduces the consumption of JavaAgent in microservice governance due to too many Groups or too many configuration items listened to by instances, and improves the maintainability and readability of the configuration. In addition, Sermant's dynamic configuration model also supports the mainstream configuration center ServiceComb Kie, Zookeeper, Nacos, which can meet the use of different microservice governance scenarios, making it more convenient for users to carry out microservice governance and operation and maintenance operations.
