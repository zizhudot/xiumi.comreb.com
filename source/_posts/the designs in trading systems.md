---
title: the designs in trading systems
description: the designs in trading systems
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

### Preface

Recently, I've been reading some books, one of which is "Enterprise Application Architecture Patterns", and I wanted to write some notes, but the time I wrote them was 03 years ago, which is a long time ago, and the structure of the system may have changed drastically, which is different, and the excerpts feel very old, and they don't have a great resonance. However, the inner peace when reading at that time is still very fond of. Turning around, I have also studied design principles, patterns, etc. before, but I mainly went to the heart and talked about my feelings. To get inner peace, I feel that it is still necessary to refer to the logic in the book, and connect with some of the understanding in daily work. So, based on these principles and part of the pattern briefly talk about.

#### Design Principles

**Single Responsibility Principle** **Single Responsibility Principle** **Single Responsibility Principle

**Definition:**

> The Single Responsibility Principle (SRP: Single responsibility principle), also known as the Single Function Principle, is one of the five fundamental principles of Object Orientation (SOLID). It states that a class should have only one reason for a change to occur.

**Case:** Different business activities have different service entrances, whether it is a fulfillment system, or a reverse refund system, there are more business processes bpm. the advantage of doing so is that it can be better divided into scenarios, different scenarios under the interface of the flow limitation, error definitions, process design, regression testing, etc. can be developed independently, and the impact of the surface is more certain. If you use a common service, which routes many sub-services, although it seems to be able to do some common operations, but the mutual constraints and constraints will be a little more, and the facets can also solve some common problems, there is no particular need. **Extension:** It is worth exploring further that although it is easier to form a consensus on the isolation of the entrance layer, further down the line, are processes, process nodes, capabilities, extensions, etc. still allowed to be shared by different scenarios? In practice, it often depends on the degree of difference in capabilities, the complexity of the scenario, and based on the comprehensive consideration of development and maintenance costs, the style of different systems is still not quite the same:

+ fulfillment system Down the line will be precipitated by the degree of competence perspective, within a competence need to consider a variety of scenarios, for example: in the fight to extend the point, may come from the confirmation of receipt, may come from the return and fight, but also may come from the deposit forfeiture;
+ The extension of the reverse refund system, as far as possible, will be in accordance with the business activities independently, such as the timeout customization of the refund, the buyer to apply for a refund, the seller agrees to refund, the buyer to return the goods, and so on, different business activities have their own extension points.

Independent, although the impact of the surface will be reduced, but the modification of the time, may be too much independence, and missed some scenes, everything is two-sided. The good thing is that when we make decisions, the scenarios we face are often specific and countable.

! [](https://pic3.zhimg.com/80/v2-5a43d6249d6cd7a920e9f2de642986ee_720w.webp)

Single Duty Principle Understanding

**Principle of opening and closing**

**Definition:**

> The Open-Closed Principle, in the field of object-oriented programming, states that "objects (classes, modules, functions, etc.) in software should be open to extensions but closed to modifications", meaning that an entity is allowed to change its behavior without changing its source code.

**Case:** Whether it is the iteration of the extension framework TMF, or the back of the star ring system proposed, an important purpose is to solve the "business and platform isolation", which is also an important embodiment of the principle of open and closed. Core logic should be controlled by more familiar platform personnel, should try to generalize, less modification; extended logic should be understood by business developers, should be as flexible as possible, easy to adjust. Look inside the system, in fact, there are many extensions of the domain capabilities, for example: payment can go straight to Alipay, you can also go through the payment system linked to WeChat and other non-Alipay channels. Such an extension, but also the embodiment of the principle of open and closed, just closer to the core process, the impact is greater. To expand the external look, even if the business APP package, product packages and other plug-ins inside, or may serve multiple industries, multiple scenarios, there may be a lot of re-routing and expansion. For example: Amoy system to serve many industries, clothing, home appliances, beauty, etc., different business customization is not quite the same, often with some strategy, chain of responsibility for the extension mode. It can be seen that each level can design its own extension mechanism. **Extension:** For the extension level of Starring Ring, there are several issues that can be further considered. **1. Business isolation mechanism** For the isolated change part, we actually have more expectations: we expect that different businesses and maintainers can still be isolated from each other. Although, inside the system of Starring Ring, the concept of business identity is used to do the isolation, but this is only a technical point of view, the scene conflict as far as possible front to the level of resolution, reducing the pressure of the subsequent implementation, the use of not very flexible. For example: when the circle of products has not been ordered "business identity", according to the commodity labeling and other identification of the circle of products, will cross multiple "business". Good in technology, there is a "product package" program, can be superimposed on the business, to achieve logical reuse, but also often appear "missing stack" scenario. But if, without providing the concept of business identity, based on the request scene judgment, then the impact of the surface and expression will be full of uncertainty, but also difficult to solve the conflict "who" and "who" conflict determination. **Boundary between business and platform **We often talk about the base domain, in fact, can be considered as the base + domain, because in addition to the domain capabilities and extensions, such as business processes, commercial capabilities, base implementation, platform share, common packages, development of SDKs, etc., we think that it is the base, and need to be involved in the platform personnel. However, there are often exceptions:

+ Inside a domain extension, providing capabilities for a particular business (which has more complete and independent logic) can be done without going to the extension point. There will be, long in the platform of the jar package, but the evolution of the rules is basically business set, the development of the platform needs to intervene in the special case of cooperation;
+ In an independently deployed system, the code base is forked out for independent evolution. At this point, the entire hierarchy is defined as "business";
+ Business capabilities are considered platform capabilities and are also integrated into the platform sar package, but things like tax and import business capabilities are basically international business maintenance and hardly platform logic.

From these examples, the boundary between business and platform has transcended the definition of specific levels, and is no longer so absolute. The core is still following the direction of "one authority, one responsibility".

! [](https://pic3.zhimg.com/80/v2-3f667a3b4bcc0b51331c1447506c4a0a_720w.webp)

Understanding the principle of opening and closing

**Richter's Substitution Principle

**Definition:**

> Liskov Substitution Principle LSP One of the fundamental principles of object-oriented design. LSP is the cornerstone of inheritance reuse, the base class can be truly reused only if the derived class can replace the base class and the functionality of the software unit is not affected, and the derived class is able to add new behaviors based on the base class.

**Case in point:** Replaceable ideas that we often use:

+ When doing extension point customization, we don't care whether it's a business package or a scenario product package that returns the result, only the result of the customization.
+ When making database switches, we don't care which data service we go to, just the results returned;
+ When making an external payment system call, we don't care whether it goes to Alipay or WeChat, only the result of the payment;
+ When making an order query, we don't care whether it goes to the order repository or an external service (such as evaluation), only the result of the query;
+ .......

The principle of substitutability allows us to program for abstractions; whether the substitution is smooth enough depends on whether our abstractions make sense. **Extension:** While we can abstract and do customizable replacements, it is often in fact difficult to make them senseless:

+ Service guarantees may not be the same: for example, pending payment, pending shipment, etc. query of the order library, pending evaluation query of the evaluation interface, the interface may not be consistent with the level of protection and capabilities. Need to do additional stability guarantee.
+ The realization of the ability may be inconsistent: after 3 months, the order will enter the history of the library, although the query level can be adapted to achieve the consumer senseless, but the subsequent operation of the consumer will be subject to constraints because of the change, the two sides of the ability is not the same. Some buttons will be downgraded after entering the history library.
+ Agreement may be inconsistent: for example, the replacement of the payment system, Alipay because of guaranteed transactions, so the sale of money back more quickly and conveniently, but WeChat Den channels, subject to the back of the funds escrow and strategy, the refund may not be so timely. The two sides of the error code, etc. is also different.
+ .......

! [](https://pic1.zhimg.com/80/v2-68877e2e80e707a022f13e90a18582e0_720w.webp)

Richter's Replacement Principle Understanding

**Dimmitt's Law***

**Definition:**

> Dimitri's Law can be simply stated as: talk only to your immediate friends. for OOD, it is further interpreted in the following ways: a software entity should interact with as few other entities as possible. Each software unit has minimal knowledge of the other units and is limited to those that are closely related to the unit.

**Case: **The process of executing a business activity is a process of coordinating data manipulation, and eventually everyone agrees to drop a library and send a message. In this process, in order to coordinate the collaboration of various areas, there is a set of basic processes and corresponding nodes composed of coordination layer, the most typical coordinator inside this layer is the context (Request, Result, Context and other concepts). Data is obtained from the context, and if it is to be passed to subsequent nodes, it needs to be stuffed back into the context, called recycling. From a larger perspective, the entry system also acts as a coordinator. For example, the ordering system calls the merchandise system, inventory system, marketing system, funding system, fulfillment system, etc. for data collection and delivery. Systems rarely call each other directly. Such coordinators often do the calling and CONVERT operations, but because of this layer of CONVERT, there is also understanding and control:

+ The model can be streamlined to reduce the number of data transfers and multiple CONVERTS in the chain;
+ Can control read-only, to avoid subsequent unintended tampering;
+ Can save performance, can design lazy loading and other patterns, and then really get it when needed;
+ ......

**Extension:** The coordinator, because it needs to carry information about the various participants, gets thicker and thicker as there are more and more participants. And because some systems have more layers, they also get tortured by layers of covert, and every time a new piece of data is added, it needs to be added all over again. So, gradually, people began to use a shared model, carrying relatively primitive data. Behind such an ending, another idea is triggered: each participant provides a fixed area to get the raw data, and playing towards the data center, is it not possible to bypass the coordinator layers of transmissions. And the data center should know how to better manage its own data. If you have a hierarchy of systems that just CONVERT, then indeed this will be much easier, but if you have some obscure process logic among your systems that may process this data, then it's beyond the scope of the data center. In addition, if you have an aggregated root design, where certain parts are one piece of the whole, the consistency of a decentralized data center can be difficult to manipulate. Finally, and more importantly, the coordinator itself is meant to coordinate, so it must be "known". Whether it is easier to find the context or the decentralized centers is also an important point for the developer, and a decentralized approach requires a certain statute.

! [](https://pic4.zhimg.com/80/v2-7c3b22e0aff1c26d5ce63ae3a3065183_720w.webp)

Dimitri's Law.

**Principle of Interface Segregation**

**Definition:**

> A client should not rely on interfaces it does not need. Dependencies of one class on another should be based on the smallest interfaces. It is better to use multiple specialized interfaces than a single total interface. A class's dependency on another class should be built on the smallest interface.

**Cases:** Common cases of interface isolation are:

+ Isolation by read/write capability: one set of interfaces for reading data, another for writing operations.
+ Isolation by operation role: one set of interfaces for buyer operation, one set for seller operation, and one set for junior operation.
+ Isolated by page type: one set for PC, one set for H5, and one set for client.
+ Segregation by component protocol: one set for Ultron, one set for Astore, one set for DTO.
+ ........

When we see these scenarios, we naturally think of isolation, and the code itself is most likely in different modules. The isolation of interfaces is not just about the way they are declared:

+ For the client, the dependencies can also be smaller (although there tends to be only one big client per system) and some unnecessary dependencies can be excluded;
+ For the server side, it can also be better developed independently, avoiding coupling, for reuse can also abstract SHARE and COMMON.

**Extension:** in the order management system, there is an interface is doOp, defined the operation of the button, through the incoming operation code is not the same, you can carry out the "reminder to ship", "cancel the order", "delete the order", "order", "order", "order", "order", "order", "order", "order", "order", "order", "order" and so on. Delete Order", "Extend Receipt" and so on. The background for doing so is that the order button may be up to hundreds of buttons, the definition of the interface, not only the server-side things, but also need to apply for wireless packaging interface mtop, the client should be inherited, in order to maximize the reuse to the client's pathway, provides a more general interface. Here, it can be seen that the principle of interface independence is not absolute, and the number of abstractions to be made, the degree of similarity between all have a relationship. In addition, the above button example is not "absolutely not isolated", just the entrance layer of reuse, the subsequent is still in accordance with the button code is strictly orthogonal, according to the button code will be routed to a different processing strategy.

! [](https://pic2.zhimg.com/80/v2-c942146f1309c3d7cfeada6210288859_720w.webp)

Interface isolation principle understanding





**Principle of inversion of reliance**




**Definition:**




> The Dependence Inversion Principle (DIP) is for programs to depend on abstract interfaces and not on concrete implementations. Simply put, it requires programming the abstraction and not the implementation, which reduces the coupling between the client and the implementation module.




** Case: ** If you think that the basic services in the short term will not change, there is no more than one set of implementation, often directly according to the call chain in the "upper dependency on the lower" logic to rely on, so it will be very concise and efficient. For example, the order management system inside the order query service, as Repo, as the underlying service, in the domain is a direct call instance. If the service is considered to be external, not subject to their own control, to isolate the changes, to retain the ability to upgrade the interface, then often another layer of interface packaging. Inside the ordering and fulfillment system there is the concept of a gateway gateway. It becomes dependent on the abstract service interface and is not aware of the concrete implementation instances. Add a layer of abstract interfaces for decoupling, will maintain a better loose coupling ability, because the interface is an abstract contract, the two sides can be developed independently, but will also bring the cost of management, which is a judgment and trade-offs. **Extension:** Although conceptually this level is good, there is still some cost to do it right:




+ packaged module: suppose in the process of A dependency B, the introduction of abstraction C. Such an abstraction layer, because and A, B has nothing to do with, should be a separate jar package and code library. But often, because of the trouble of creating new libraries, they will be hosted in a submodule of A or B, and need to be typed separately when packaged, which is rather awkward.
+ Complex Object Challenge: Abstract oriented interfaces means more CONVERT, which may be relatively easy in a normal system, but in the context of trading complex object design, it will be a painful process again. To add to the misery, the domain objects of a trading system are a logical mapping to the database model, and it is hard to find out how the data is brought out after overlaying these layers.




Therefore, sometimes, it will be reversed, choose a tightly coupled model, in a complex system, there is often such a feeling: simple, pure, tightly coupled is a dawn, because point and click, you can find the relevant code, rather than point and click ....... Point and click and get lost. Said so, is not singing the opposite, I hope to be able to dialectically look at the problem, combined with specific scenarios, there is a give and take, there will be a loss.




! [](https://pic4.zhimg.com/80/v2-b5327f01830a3161a8205dccd9061b27_720w.webp)




Dependency Inversion Principle




### Design Patterns




Here is a selection of some of the 23 design patterns to give a little introduction.




#### **Template




The template approach is said to be an abstract decomposition of an execution process, complete with a standard body logic and extensions through skeleton and extension methods.




! [](https://pic3.zhimg.com/80/v2-d7de768b1cac8961e070935009eb5636_720w.webp)




It is more appropriate to make analogies to the design of platforms and extensibility on the transaction chain. The basic template is the entire process of choreography and the corresponding nodes, and the extensible place is a variety of business customization area. This forms a better integration of platform and business.




! [](https://pic1.zhimg.com/80/v2-07a0268c518f643bb20d3f61a324ebbc_720w.webp)




#### **Chain of Responsibility**




Chain of Responsibility means letting the requests be executed one by one by the processors in the queue until a willing one is found.




! [](https://pic3.zhimg.com/80/v2-1865b12150f07ee5458095c9400ecd0e_720w.webp)




Business Capability Extension, Domain Extension, traverses the implemented plugins and combines them with the recycling rules to perform a timely meltdown when executing the recycling result. This is similar to the logic of the chain of responsibility. Take the example of "whether to skip notification payment" when confirming receipt of payment, TMF execution engine will traverse the implementation of product packages and app packages, and when it finds the first result that returns to true (skip), it will stop the execution, and the whole returns to true.




! [](https://pic2.zhimg.com/80/v2-478e925b168c279d3afbc1058ebdf7b9_720w.webp)




#### **Strategy**




Strategy means that there are different algorithms for accomplishing a thing, which can be switched relevantly.




! [](https://pic4.zhimg.com/80/v2-299d128be44773987b7ddb368205bab7_720w.webp)




In reverse refund, there is a need to support different refund links, some need to be secured transactions, some are margin links, some are microsoft payments, some are card and asset refunds. In order to support multiple outgoing strategies, a policy model is used, which allows you to customize various funding strategies through extension points, while executing a single one or multiple ones.




! [](https://pic3.zhimg.com/80/v2-8db87e1b58ae44a561b3c4df82800e6e_720w.webp)




#### **Observer**




The Observer pattern is said to be the collaborative mechanism by which we accomplish change notification by registering, dropping back such a collaborative design.




! [](https://pic4.zhimg.com/80/v2-2a1adccb0f988e136eeaf334cb91d00f_720w.webp)




The intra-system observer pattern is not seen much in transactions. But there are still many inter-system message-based observer patterns. The more typical ones are reverse 0s refund: the fast consent function of 0s refund is realized by listening to the message created by the refund and making the consent call. Through the asynchronous notification method of the message, it can be better decoupled, and also can utilize the message rerouting mechanism in case of failure to increase the probability of success.




! [](https://pic3.zhimg.com/80/v2-9fe7a10233afa317f078a04a5bf0ad82_720w.webp)




#### **State**




State mode means that in different states, there are different processing behaviors.




! [](https://pic3.zhimg.com/80/v2-242cb672c97846b40465c02456e0222e_720w.webp)




A workflow introduced in a trading system will define the states that a business activity can go through and the operations that can be performed in each state. For example, a normal guaranteed quasi-transaction flow contains the following state nodes: creating an external payment transaction, payment callback, creating a logistic order, shipping the goods, and confirming receipt of the goods. Each node also defines what operations can be performed, for example, in the Create External Payment Transaction node, you can perform payment verification, close the order, modify the price and other operations, but you can not make payments, refunds and other operations, because there is no payment.




! [](https://pic2.zhimg.com/80/v2-4e57e9d1dde80ba7b5a98fbb1d92467d_720w.webp)




#### **Mediator**




When multiple classes are to be coordinated with each other, mediators are often introduced to coordinate and reduce the cost of knowledge for everyone.




! [](https://pic3.zhimg.com/80/v2-cab45a874a3b186dac1530967a660efa_720w.webp)




During the execution of a process in a trading system, there is a large context which coordinates data from various domains. One of the more typical scenarios is that each orchestration node may affect a data update that needs to be stored somewhere and given to the final update node. This role of transferring information often falls to the context as the intermediary. Here is a rough structure of update collaboration in a reverse process.




! [](https://pic3.zhimg.com/80/v2-fa91c36c29121b0cfa1e1f313955d482_720w.webp)




#### **Combination (Composite)**




Composites can recursively describe a hierarchy of objects through patterns of inheritance, and child nodes.




! [](https://pic2.zhimg.com/80/v2-43ad9ff1628c043e02831f7e0e6ccea5_720w.webp)




A better understood example of the idea of recursion is the splitting of orders in an order placing system, where a number of columns of orders are grouped together, over and over again. Understood logically, it is like recursively going to further refinement.




! [](https://pic4.zhimg.com/80/v2-8bbc268b5681827d255b9919f999011b_720w.webp)




#### **Single piece (Singleton)**




Singleton means to make sure that the object is created only once, as a unique resource, in a multi-threaded situation.




! [](https://pic1.zhimg.com/80/v2-a1d8c7ca77a0658e3812f9bcecb6e348_720w.webp)




In the Order Management System, externally invoked services are named Repo, as a repository. In order to easily access these repositories, they are accessed through the singleton pattern, so that some tool classes can also easily call the service through static methods without injecting beans. such repos are: order service, evaluation service, icon service, timeout service, etc.




! [](https://pic1.zhimg.com/80/v2-d003de3baab7e31ddf459fd5ceceeef8_720w.webp)




#### **Interpreter**




Interpreter is said to form a set of language for a set of contexts that can accomplish corresponding tasks by interpreting the meaning of expressions.




! [](https://pic2.zhimg.com/80/v2-8647e81522dc3f6633a358d6bb1614e5_720w.webp)




Interpreter mode seen in the transaction is mainly, the original Tao system of Newton system, a dynamic script class configuration. This configuration platform mainly addresses some of the dynamic rules in the product package, and through the push mode, the dynamics of interpretation can be utilized to reduce some of the deployment costs.




! [](https://pic4.zhimg.com/80/v2-4357ef67fef9e86029d8f95c9e2b60a3_720w.webp)




#### **Proxy**




Proxy is to package a class to forward the related operation twice or to do some control.




! [](https://pic2.zhimg.com/80/v2-78e7acdded1e601908866c95a563c7e1_720w.webp)




In the order management system, there are certain protection measures for the context in order to avoid the context being tampered with by various domains. When entering the specific execution node, the context will be converted, the conversion process, through the packaging of read-only interface, to proxy entity objects, to provide read-only services, and can not get a specific instance, can not be set to modify.




! [](https://pic1.zhimg.com/80/v2-911b762a30564071365cb56bf1b34814_720w.webp)




### Summary




The Enterprise Application Architecture Patterns has a better written description of patterns:




> Each pattern describes a problem that keeps recurring around us, and the core of the solution to that problem. This way, you can use that solution again and again without having to do duplication of effort.




This article draws on some principles and design patterns to talk about some of the designs in trading systems that I have peeked into. Hopefully, it will give you a perspective and a little more insight into the trading chain as I see it.