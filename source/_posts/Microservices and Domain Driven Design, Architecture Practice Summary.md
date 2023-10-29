---
title: Microservices and Domain Driven Design, Architecture Practice Summary
description: Microservices and Domain Driven Design, Architecture Practice Summary
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

> What kind of architecture is worthy of build-to-fly changes?



## I. Software Complexity



## 1. Reasons for Complexity



If there is a continuous iteration cycle of the software system, then the complexity of the business, technology, architecture will straighten up, the corresponding development difficulty will also increase, can be summarized in one sentence the root cause: the only constant is change;






+ Business changes: the root cause of complexity, in the process of multi-version adaptation of multi-end code rapid expansion;
+ Data changes: data with the changes and development of the business, the accumulation of continuous precipitation, the need to do horizontal and vertical management;
+ Technical upgrades: technical components may be due to loopholes, or a better solution to the problem, uninterrupted upgrade version;
+ Personnel changes: Once the module developers have turned over, the change of hands will bring differences in the style of the code;
+ Mindset ups and downs: Continuously responding to complex problems, but a smooth mindset is hard to sustain and is a factor in staff turnover;



Responding to complex changes has always been the core of the software engineering difficult problem, how to use smaller architectural changes to cope with larger business changes, is often said in the design: high cohesion, low coupling; also need to add a very important point: from the technical level alone is unable to continue to solve the complexity of the problem, but also need to define the process from a management perspective standardize a variety of solutions is the whole department to continue to face the matter.



## 2, dealing with complexity



Whether it is often referred to as design patterns, principles, object-oriented, or architecture commonly used clusters, microservices, domain-driven, etc., are seeking a more reasonable program to respond to changes in the business; but there is no once-and-for-all solution to do a certain degree of forward-looking design to anticipate the business, but also to avoid excessive design impact on the progress of the business; this requires the R & D team to have a certain degree of business height and technical This requires the R&D team to have a certain level of business and technical depth:






In the process of system implementation, the need for in-depth analysis and understanding of the business, and constantly optimize the technical level of the solution; for example, the idea of microservices is to achieve low-coupling between the business blocks by means of split, domain-driven design to achieve a high degree of cohesion of the various business logic; the following practice around the two ways to go to a detailed analysis.



## Two, microservice architecture



## 1. Architecture Design



System architecture design is an extremely complex thing, in the work of these years have experienced the following stages: single-service, multi-service clusters, microservices, continuous integration; in the last 2 years the more stable selection of microservices + automated integration model:






Think about the logic of its essential changes, that is, in order to cope with more complex business systems; regardless of the business split or model design, are constantly realizing the principle of **high cohesion and low coupling**; to reduce the impact of the correlation between the business, separating the business and the high degree of coupling of technology.



## 2. Business Scenarios



Here we first look at a classic business scenario: e-commerce transactions; based on the microservice architecture of the e-commerce transaction scenarios, usually involves at least the following core services: transactions, accounts, orders, commodities, warehousing, logistics;






Standing in the business perspective, modular split and management, combined with continuous integration of components, usually can easily cope with a variety of complex business scenarios, but there is no real sense of once-and-for-all means of business changes brought about by a variety of problems will always be brainless to promote the development of a more reasonable solution to find;






In a complete e-commerce trading scenario, in fact, the real microservices involved are far more than just a few in the figure, intertwined in the Trade service associated with a number of other services, in the MVC hierarchical management, there will not be a greater risk at the initial stage, but the business, once after the upgrading of the transformation of the multi-version and the existence of version-compatible requirements, it will give people a feeling of extreme confusion and insubstantiality;



If the comprehensive ability of team members is high, and version has enough time to design and optimization, this problem can be properly resolved, if there is a time-critical task heavy situation, the ensuing ** pressure will continue to be in the development and testing between the back and forth across the jump **;



Solve the related business scenarios of R & D know that refactoring plus continuous integration capabilities, combined with rigorous testing, can cope with the constant changes in the business; but in the process of version compatibility will still lead to the expansion of the code in the project to the fly, especially the emergence of a mid-field replacement, will allow the personnel to take over the situation in the buried and leave, resulting in a drastic struggle of the mind.



## 3. Problem Analysis



In the MVC architecture model, the project usually carries out the following layered management: control layer, service layer, persistence layer, storage layer; the service layer in a specific complex scenario will do the refinement of the split, such as third-party docking, the secondary packaging of commonly used middleware:






For the players in the complex business line to compete in the Mvc layered model of the defects are well aware of the Service layer to focus on a large number of complex logic, usually the core business block there will always be a few lines of code over a thousand lines of implementation logic, regardless of what ideas and models to split encapsulation, it is very difficult to solve the layer of the expanding expansion of the expansion of the problem.



## 4, process-oriented



In MVC layering, the process code is extremely obvious, usually based on database tables and relationships, mapping and building related entity objects, these entity objects do not have specific behavior and logic, just as the carrier of data and structure:






From the object-oriented definition of the class to see: attributes and behavior; and in the MVC model, the majority of entities are just as the structure of the data definition of the in- and out-references, which can be understood as data containers, in the MVC between the layers of the constant handling and processing.



## Third, domain-driven design



Compared to the MVC layered design, Domain-Driven Design (DDD) for the realization of complex business systems, proposed a more reasonable solution, the DDD model involves a lot of terminology and abstract concepts, you can refer to `EricEvans` books, this article only describes the core concepts in practice.



## 1. Separation Model



DDD model in the layered design, divided into four core layers: access layer, application layer, domain layer, infrastructure layer; note that this is simply standing in the service side of the conventional architectural perspective to look at, it is clear that the separation of MVC pattern in the service implementation layer of logic:






The domain layer is the key to encapsulate the complexity of the business, the application layer to provide core support for business management; the whole model is also more vertical thinking, effectively alleviate the phenomenon of single-layer complexity over the phenomenon; from the model design alone, in the project based on the layering to manage the code packages, but also to make the design of each layer more clear and independent.



## 2. Design Ideology



Domain-driven design is not a simple hierarchical management model, involving a lot of abstract logic and terminology, such as: domain, bounded context, entity, aggregation, value objects and so on;



**2.1 Domain



Domain can be understood as an ensemble of problems to be solved in a business scenario, a constraint with scope and boundaries; the domain can be split into multiple sub-domains, which are usually described as: core domain, support domain, and generic domain:






Regarding the division of sub-domains is also with reference to business attributes, the core domain can be understood as the most critical business scenarios and requires resource tilting to cope with its continuous development; the support domain can be understood as a relatively stable business; the generic domain is biased towards public capabilities at the level of the system architecture; the realization of business partitioning through the splitting of domains is in line with the idea of the splitting of microservices, and the two models are relatively unified from the business point of view;



**2.2 Boundary Contexts



One of the most obscure abstract concepts in DDD, the application of boundaries to a particular model, can be understood by borrowing an analogy from the original text: a cell exists because the cell membrane delimits what is inside and what is outside the cell, and determines what substances can pass through the cell membrane:






The definition of bounded context involves the idea of granularity, that is, each granularity should have independence; such as the above figure warehousing business, the deployment of services and warehousing sub-domain, warehousing context can be made into a one-to-one correspondence, or in the warehousing sub-domain were defined: warehouses and shelves in the context of the two; here there is a great deal of flexibility, and there is no real standard can be referred to.



**2.3 Mapping Relationship**



Do a good job of delineating the boundaries of the context, clarify the relationship between the various contexts, and clarify the order of dependencies in the business scenario, so that you can better promote the development process to the ground; for the description of the relationship between the context is also much more than just these diagrams, there are also shared kernel, cooperation and so on:.






+ Upstream and downstream (U-upstream, D-downstream): describes the relationship when the context is invoked, the service invoker is D, and the service provider is U;
+ Anticorruption-Layer (ACL for short): a layer that encapsulates the context when it interacts, providing checksums, adaptations, transformations, etc. for actions;
+ Open-Host-Service, Published-Language (Open-Host-Service abbreviated OHS, Published-Language abbreviated PL): defines the access protocol;



During context interaction, the preservation layer can maintain context isolation and independence, ensuring that the caller does not directly depend on the service provider, thus realizing dependency decoupling between different contexts; at the same time, this can also lead to a large number of object transformation actions;



**2.4 Modeling Design**



Sub-domain and boundary line contexts complete the splitting and chunking of the business so as to carry out partitioning; based on the antiseptic layer to reduce the coupling degree of each boundary context; the aggregation idea ensures the solution cohesion of the business problem; the strict layering model realizes the decentralization of the service support capability;






+ Anticorruption-Layer (Anticorruption-Layer): a layer that encapsulates context interactions;
+ Domain-Layer: a layer responsible for the design and implementation of domain logic in a layered architecture;
+ Domain-Service: encapsulated to a domain service when the behavior does not identify the attributed entity;
+ Aggregate: a collection of related objects describing the core domain, often using the aggregation as a unit for data modification;
+ Entity (Entity): an object defined by an identity, not based on attributes, e.g. Uid identifies a user entity;
+ Value-Object: an object that describes features or attributes but has no identity;
+ Factory (Factory): encapsulates the complex creation logic and types of objects;
+ Repository (Repository): the storage, caching, search and other resources encapsulation mechanism, corresponding to the domain model;



Domain model of the core pursuit of the goal: high cohesion, low coupling; more abstract, complex design ideas, also means that the implementation of the landing is more difficult, but it can not be denied that the domain model as a solution to complex business, the logic is indeed more reasonable.



## 3. Engineering Practice



In the practice of code engineering, the domain model can integrate different sub-domains into their respective services, and can also be isolated and maintained by multiple modules (Module) in a service, i.e., a module corresponds to a bounded context;






The isolation of business issues in the form of sub-modules, sub-layers and sub-packages is a basic means of code engineering, here is just a description of the organization, in the actual development, according to the dependency order of the class libraries to unpacking management;



In the execution process of the program, not all interactive commands need to go through the domain layer, in fact, most of the query commands in the business are more than the add, delete and change commands, so in the pure read data request, the application layer can bypass the domain layer to directly access the infrastructure layer, reducing a layer of data processing logic.



## IV. Practice Summary



Finally, to discuss some architectural practice experience, with the continuous development and upgrading of technology, to solve business problems provides great convenience, whether it is a single service in a variety of mature components, or distributed microservices system, or focus on the business management of the domain model; each architectural selection has its own applicable scenarios, and different selections mean that different realization costs;



In fact, when doing architecture selection, mature and experienced leaders, are extremely good at making compromises, that is, often referred to as a step back to broaden the sky; usually need to take into account the team's comprehensive level of business needs and product design, of course, in the actual collaboration process of multiple parties are required to make relative concessions, but the quality of the requirements of the core business as well as the realization of the logic can not be discounted.
