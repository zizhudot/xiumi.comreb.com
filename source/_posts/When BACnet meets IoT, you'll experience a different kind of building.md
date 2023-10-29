---
title: When BACnet meets IoT, you'll experience a different kind of building
description: When BACnet meets IoT, you'll experience a different kind of building
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

## Introduction



In the 14th Five-Year Plan, "new infrastructure" is undoubtedly a key area of concern. I believe we are all familiar with the term "new infrastructure". Compared with traditional infrastructure, the biggest difference between new infrastructure is the embodiment of the characteristics of the digital economy, which contains several features such as digitalization, networking and intelligence. There is no doubt that the new infrastructure will become a powerful support for the construction of new smart city facilities. Smart venues, smart buildings as the most important part of the smart city, in today's view is no longer some strange words. In fact, almost all large commercial complexes, office parks are equipped with comprehensive building automation, security, office and other integrated systems, of which the building automation system (BAS) is the most important and indispensable part of the smart building.



! [](https://static001.geekbang.org/infoq/47/47ed7a921947030258393aed656bcf77.jpeg)



The use of building automation system can realize unified monitoring and management of all utility mechanical and electrical equipment of the whole building. The system can seamlessly interface with various building facility subsystems, including central air-conditioning system, water supply and drainage system, power supply and distribution system, lighting system, etc., to further implement automated control management and optimization of the equipment. For example, the system can monitor hidden dangers that cannot be detected by human beings in time, avoiding major accident losses; automate indoor thermostat/humidity control and humanized lighting control to enhance the office experience; and optimize the automated control and management of the equipment to reduce the equipment failure rate and operation and maintenance costs. Based on the comprehensive optimization of multiple use scenarios, it aims to create a safe, efficient, comfortable and convenient space environment.



## BACnet Introduction



Next, let's talk about some of the technology behind the building automation system. As the "control core" of intelligent buildings, BAS faces a variety of subsystems and devices such as lighting, cooling, heating, etc. For the same category of devices, there are also differences in different manufacturers, models, and interfaces, which makes the complexity and realization cost of BAS system very high. In order to reduce the complexity, the industry generally introduced a number of building automation protocol standards, of which the BACnet protocol is undoubtedly the highest degree of attention and acceptance, the following we introduce the basic situation of the protocol.



BACnet known as A Data Communication Protocol for Building Automation and Control Network (Building Automation and Control Data Communication Protocol), is organized by the American Society of Refrigeration, Heating and Air Conditioning Engineers in June 1995 to develop a building automation network communication protocol, the standard will be different manufacturers of equipment into a single building automation network. The standard will be different manufacturers of equipment to form a consistent self-control system, designed to solve the different manufacturers of equipment between the interoperability (Interoperability) of the demand. BACnet protocol contains equipment data communication and command and control two parts, and based on these two parts of the design of the relevant communication standards.



! [](https://static001.geekbang.org/infoq/8d/8daa07c59c8369963eb5275b60fbdc12.png)



#### BACnet Protocol Layers



BACnet is a simplified 4-layer network protocol structure, including physical layer, data link layer, network layer, and application layer, as follows:



! [](https://static001.geekbang.org/infoq/29/293da6b71d6d765fb47ed52cd4b92b43.png)



Figure - BACnet 4-layer protocol structure



[ Explanation



+ Physical Layer : Provides the physical connection between devices and the means of transmitting carrier signals.
    
+ Data Link Layer : Abstracts and converts the physical signals into data frames, which are propagated by means of frames (Frame) or packets (Packet). This layer is responsible for the access and addressing of the communication medium, error correction and flow control functions.
    
+ Network Layer: Realizes the local network or cross network for routing and transmission of messages, and is responsible for network packet sequence/traffic/error-checking capabilities.
    
+ Application Layer: defines the communication semantics of the BACnet protocol, including answer/non-answer packets, and communication of BACnet standard objects/services. The application layer is the most important part of the protocol standard design, and is also the most concerned part of BACnet application development.
    



The BACnet protocol unifies the application layer and network layer parts, and provides seven combination schemes in the physical and data link layer parts. Among them, two LAN networking based on BACnet IP/Ethernet and BACnet MSTP/RS485 are the most widely used in building automation scenarios. BACnet IP allows communication across subnets/area control systems and takes advantage of fiber optics and Gigabit Ethernet to achieve IP addressing of devices.



#### BACnet Network Topology



In the BACnet network layer definition, a network is a localized network consisting of one or more network segments interconnected by repeaters or bridges with a single local address space; in a BACnet network, the network layer implements global to local address translation and addressing.



The following is a typical BACnet network topology:



! [](https://static001.geekbang.org/infoq/ba/ba4e820c088ad1a1dc3f012407637682.png)



Related concepts



+ Physical Segment: A section of physical media that directly connects some BACnet devices.
    
+ Segment: A network segment formed by connecting multiple physical segments through repeaters at the physical layer.
    
+ Network: Multiple BACnet segments interconnected by bridges, each BACnet network forming a single MAC address domain.
    
+ Network: Multiple networks using different LAN technologies are interconnected using BACnet routers to form a BACnet network. In a BACnet network, there is exactly one message path between any two nodes.
    



BACnet networks have obvious LAN characteristics, and BACnet router nodes can connect BACnet local networks to external networks (e.g., Ethernet, ARCNET). In addition, the BACnet network layer defines a clear packet protocol (NPDU) specification and supports unicast, multicast, and broadcast functions for packets.



#### BACnet Application Interaction



Since the BACnet protocol utilizes only a simplified four-layer structure, the BACnet Application Layer Protocol needs to consider end-to-end reliable transport in addition to application layer services. An APDU (application layer packet) contains two parts:



+ Protocol Control Information (PCI), which is a fixed header and contains the APDU type (service request/response), message segmentation reorganization information.
    
+ User data, which is variable and contains information specific to each service request and service response.
    



The BACnet application layer supports two modes of interaction: "request-response" and "request-no-response".



The BACnet application layer supports both "request-response" and "request-no-response" interaction modes. [](https://static001.geekbang.org/infoq/62/62ad05c67c6df269d0d04dd42fe664c7.png)



Figure - Request-Answer Mode



Figure - Request-Answer Mode [](https://static001.geekbang.org/infoq/a5/a5818f9161b0eb1514e67a928c49c0e8.png)



Figure - Request-Non-Response Mode



#### Objects and Services



BACnet draws on object-oriented thinking in order to achieve a language abstraction for communication between network devices.



To gain further appreciation, we need to understand the following concepts:



+ Objects, describing analog inputs, outputs, or program modules, etc. BACnet devices contain one or more objects, and devices interoperate with each other by reading/modifying the properties of the objects.
    
+ Properties, which describe the underlying fields of an object, e.g. for a sensor input object, Present\_Value is one of its properties.
    
+ Service, which describes the method by which the object operates, e.g., accessing a property of the object, implementing alarms or notifications based on the object, etc.
    



In short, an object provides an abstract description of the "network-visible" part of a building automation device, and a service provides commands to access and manipulate this information.



All BACnet objects need to contain the following common attributes:



1. an ObjectIdentifier, which uniquely identifies the object in the device. The ObjectIdentifier consists of a total of 32 characters (consisting of the 10-bit ObjectType and the 22-bit InstanceNumber)
    
2. ObjectName, a BACnet device broadcasts the object name of an object it contains to establish a connection with devices that contain the object in question. 3.
    
3. object type (ObjectType), different types of objects have a separate set of attributes.
    



The relationship between objects, services can be described in the following diagram:



! [](https://static001.geekbang.org/infoq/a3/a33c331e6a1ea44d1c581bdf88cfe77a.png)



Figure - BACnet objects, services



[ Explanation.



+ The BACnet protocol requires each device to contain a unique "device object", whose properties can be read to obtain full information about the device.
    
+ BACnet devices contain multiple analog input/output objects whose properties (current values) are used to indicate sensor/controller points.
    
+ BACnet program communicates with the device via BACnet services, e.g. ReadProperty service to read point data.
    



#### Built-in Definitions



BACnet has a set of standard objects and services built-in, which continue to be updated in parallel as the protocol evolves. The current number of built-in objects in the protocol is over 49, some common ones are listed below:



! [](https://static001.geekbang.org/infoq/70/7004c6b8b57f3136cc8b1f90f3ea940e.png)



BACnet built-in services are categorized into 6 main categories.



1. Alarm and Event Services provide the ability to notify internal attributes or state changes. 2.
    
2. File Access Services (File Access Services) provides methods for reading and writing files. 3.
    
3. Object Access Services (Object Access Services) provides methods to read, modify, and write attribute values, as well as add and delete objects.
    
4. Remote Device Management Services (Remote Device Management Services) provides maintenance and fault detection tools for BACnet devices.
    
5. Virtual Terminal Services (Virtual Terminal Services) provides a character-oriented data bi-directional interaction mechanism. 6.
    
6. Network Security Services (Network Security Services) provides peer entity authentication, data source authentication, operator authentication and data encryption.
    



## Problems with Traditional BA Systems



Through the previous introduction, we have already had a certain understanding of BACnet and BA system. It is not difficult to find that BACnet is still a gateway protocol mainly for local/LAN networking. In the field of BA, most of the devices are static, that is to say, they do not move frequently in space, which essentially fits the characteristics of building architecture. Today, although the application of BACnet has been spread throughout a variety of large-scale building systems, but most of the BA system there are still a lot of problems, which are mainly reflected in the following aspects:



**Systems are complex and not easy to deploy**



First of all, there are many types of BA systems, a BA system can contain many subdivided subsystems and sub-equipment, the subsystems integration and deployment of high difficulty; large buildings have a high degree of spatial design complexity, resulting in the overall hardware and software wiring design and implementation are very complex. It is difficult to realize wireless based on traditional solutions, and cannot be deployed quickly.



**Operation and maintenance model backward **



The traditional operation and maintenance model is relatively backward, most still rely on manual inspection, the overall efficiency of the strong dependence on human input and professional skills.



**Operational Inefficiency, Waste of Energy Consumption



Most of the traditional BA systems only achieve the "remote control" function of the equipment, lack of data integration and analysis capabilities, and are unable to fully explore the value of the data; it is difficult to realize the analysis and optimization of the energy consumption of the equipment/space.



**Closed system and data silos



In the process of long-term market competition, traditional BA manufacturers have formed their own technical barriers to self-contained system. The subsystem application protocols are diversified and privatized, and the system data is seriously siloed, further increasing the system complexity and difficulty of use.



## Huawei Cloud Facility aPaas



#### Facility aPaas Architecture



Huawei Cloud currently launches the Facilities aPaas service, which uses Huawei Cloud IoT device access and the IoT edge cloud engine as a base to construct building