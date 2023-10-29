---
title: The Elephant's Turn - How Platform Architecture Embraces Business Innovation
description: The Elephant's Turn - How Platform Architecture Embraces Business Innovation
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

### Introduction


**This is an architecture practice and inspiration session. **If you are in charge of the architect** of a **mega-complex platform (e.g. e-commerce, payment, logistics) and facing various technical liabilities (e.g. architectural complexity, team collaboration complexity), **while the business is facing the transition from platform services, to scenario-based innovation**. Then this article might be fruitful for you:


1) How to reinvent a 10 year old funding platform architecture?


2) How to build a shared platform architecture to drive a shift in the R&D collaboration model and improve global R&D efficiency?


3) How to build a flexible innovation growth architecture on top of the platform to reduce the opportunity cost of innovation?


### I. The Origin of Funding Innovation Platforms


#### **1. What is a funding business platform? **


Support for various types of business scenarios ** funds transaction processing platform **, such as a transfer, a red packet behavior (send, receive, return), etc., seemingly simple actions, behind the scenes is actually very complex. First, the complexity of the business, in addition to acquiring transactions, all other can be categorized as funds class transactions, so it carries a very complex business model. Second, from the point of view of the transaction itself, is responsible for the transaction elements of the aggregation, capital flow processing, and finally complete the fulfillment, is the payment system in the top and bottom of the core system.


! [](https://pic3.zhimg.com/80/v2-7da67587bc5b0a55c36613e2e808910e_720w.webp)


#### **2\. Platform to Scenario-based Innovation Transformation**


Funding business platform has gone through several stages of evolution: from the beginning of the monolithic application, to the service-oriented and platformized in the process of rapid development of the industry . In the recent years, it is the stage of rapid transformation from tool-based products to scenario-based services, how to support innovation and trial and error in a "stable and fast" way is a great challenge to the architecture design.


The following is a summary of the architecture design. [](https://pic1.zhimg.com/80/v2-642e709ab993e464b6928d59f2b6cce0_720w.webp)


### Problems and contradictions


#### **1\. Apparent Contradictions


**The demand for rapid trial and error of innovative business, and the contradiction between the R&D delivery cycle The threshold of funding innovation is high, and the R&D delivery cycle generally starts at 1 month, and relying on blindly adding people has failed to solve the problem of overall delivery efficiency**, the application architecture of the funding platform has historically followed the products and lines of business, forming a three-legged tripod, and a hundred contending situations (personal, commercial, and group), and with the scaled-up innovation, the business boundaries were gradually broken, and capabilities were duplicated and of uneven quality. Although the team once massively expanded, due to platform liabilities as well as complexity, it was unable to stimulate the motivation of innovation at the business level, resulting in a light head and a heavy foot, unable to take a step forward.


#### **2\. Definition of the Problem **


#### 2.1. Chimney-style funding platform architecture


**High marginal costs and duplication of capabilities** The chimney architecture of funding platforms has managed to be flexible and locally optimal in historical times. However, with the business development and integration, the cost paid by duplicated construction is far greater than the advantages brought by the flexibility, such as TOB's enterprise payment on behalf of the company, and TOB's small wallet, the bottom is based on the innovation of the shared account, all the account asset model, in and out of the gold process, need to build a set of research and development of a huge investment, obviously not in line with the intuition of the business side.


! [](https://pic3.zhimg.com/80/v2-51d24623f0266fb9dc449f7c7e6c0cce_720w.webp)


**2.2. Internet Growth Oriented Architecture**


Alipay is good at making payment platforms, but in the face of Internet-oriented scenario construction and business scale growth, it lacks relevant architectural experience, and its R&D efficiency is dragging its feet to keep up with the rhythm of innovation and trial and error.


! [](https://pic3.zhimg.com/80/v2-e534d71d667e4626897d6ca3eccce9b6_720w.webp)


### Third, the overall design


#### **1\. Architecture Vision**


This past year, we launched the Funding Innovation Platform project cluster. Defined the architecture goals of a unified platform for funding business innovation, present and future, for the larger funding domain. In terms of global design, we have a vision to design the next generation of innovation-oriented funding platform architecture, so that the funding platform will be more stable and business innovation will be more focused on the delivery of innovation logic, and the following figure shows the direction of the first version of the design at the beginning.


! [](https://pic3.zhimg.com/80/v2-c3aaa24f38c3193a7abbfd607a90c0e2_720w.webp)


### Fourth, the key architectural design (a) remodeling the funding platform architecture


**Funding platform (fund-type transaction processing), is the root node of all fund-type business innovation**, supporting the growth of all business capillaries at the upper level. However, in the past practice, due to its "three high" characteristics (high technical entry threshold, high implementation complexity, high stability requirements), the delivery cycle is too long, most of the front-line business of the "payment team" of the first impression, seriously restricting the innovative business expansion. Expansion. Based on the governance of the stock structure liabilities (multiple sets of chimneys), as well as judging the future scenario-based innovation, the demand for the efficiency of the delivery of funding capabilities, we have done an overall restructuring of the funding domain architecture. The overall unfolding is divided into three phases:** domain model refactoring, platformization design, and capability productization design. **


#### **1\. Domain Model Refactoring **


**1.1 Summarize domain boundaries through use case analysis **


The domain has to define clear boundaries, i.e., find a suitable demarcation line (what to do and what not to do) to decouple the complex relationships between domains. The key is to identify "conceptual classes" - nouns (domains), adjectives (capabilities), verbs (relationships) - from business use cases. The next step is generalization and clustering to disaggregate into different domains for the purpose of independence and reuse. **Define the domain boundaries between services** Analyze the red packet business as an example (see below): red packets, through a variety of ways to play, pass red packets between friends. Funds, driven by red envelopes, the transfer of funds between the recipient and the payer. After formatting through the requirements, we can easily identify two types of independent domains and capabilities, and in accordance with the reusability, the ability to handle funds is abstracted and sunk so that it can be reused in other scenarios.


! [](https://pic4.zhimg.com/80/v2-3a4546b1b9956d4e635b50ec06d306f3_720w.webp)


**Define the domain boundaries within the service **The essence of the funding domain is to transform the fund transfer demand of the upstream weak transaction type business scenario into a funding order, and to use the order as a carrier for the business behavior payment and asset settlement services around people, accounts, business entities, and business assets as participants. **Money business domain modeling L0:** From a complete business perspective, the **coordination capabilities** of peripheral dependencies and services (cashiering, payment, charging, restriction of rights, security, etc.) are abstracted into reusable **domain components**, and the contractual parameters understood in the order domain, the directive parameters of the source upstream, and the invocation parameters to the downstream system are integrated in the component dimensions, to support the various business activities;


! [](https://pic1.zhimg.com/80/v2-bcd4e0f8966df5123743d7239d2d3cd8_720w.webp)


**Funding Core Domain Modeling L1:** Split the previous funding order model into **funding order, funding flow model**, make explicit the difference between the funding order flow related to business actions, and the funding flow related to fund settlement; at the same time, abstract the **participant, business asset model**, and finally, provide funding participant extension, business asset extension, flexible scheduling of the funding order flow, and flexible scheduling of the funding flow capabilities to address the complexity of funds processing and enhance future scalability.


**1.2 Reasoning about Unified Transaction Models through Deduction**


Can transaction models be reused across industries? In addition to the past-oriented generalization, there is also a need for the process of deduction, that is, through the business lifecycle, and business elements, stand in a more business macro perspective to put forward assumptions and reasoning, for example, we assume that the various different types of order transactions, essentially, is the carrier of the collocation of the participant, the asset and the payment, and that this carrier can be combined by different transaction models. Or go back to the business use case of red envelopes just mentioned, red envelopes behind this business represents a multi-stage order flow transaction model, this transaction model can also be reused in other business scenarios, such as nail transfer (need to be confirmed by the payee to receive payment). In addition, we serve a large number of B and C scenarios, precipitated by B2B (**single, pooled**), B2C (single, batch, multi-stage), C2C (single, batch, multi-stage), B2B2B and other transaction patterns, it can be said that the payment system is one of the most complex transaction patterns. **One of the core pain points of the old code, unreasonable design of the order domain model **The old system for the above different transaction scenarios of the domain model design is more rigid: such as batch, two-stage, single, in the code design of the system, the corresponding model and code design are chimney-type design, resulting in the inability to digest the business innovation on the demands of the new transaction model. The order model needs to take on more downstream coordination capabilities, and the chimney-type design of the domain model brings more complexity to the 'coordination' work, making it impossible to reuse the coordination capabilities themselves. **Remodeling the order domain model: abstracting out commonality and taking advantage of the polymorphism of the java language** We take the commonality part of the domain model and abstract it into a base order model, on which transaction patterns such as parent-child orders and batch orders are used as derivatives or implementations of the base order model. This allows the downstream coordination of the domain model is no longer understood to the specific transaction model, so that changes and additions to the transaction model of the impact on the system has been effectively controlled.


! [](https://pic3.zhimg.com/80/v2-bb79ade486a6b28131d64b48a48cb082_720w.webp)

#### **2\. Platformized Design - Shared Business Platform**

Platformized design, the purpose is to achieve the maximum degree of asset reuse, and can be flexibly extended through configurations. Generally, teams based on platformized design are able to complete the delivery of complete requirements based on interface contracts, such as commodity platforms and fulfillment platforms for e-commerce, and acquiring platforms, payment platforms and billing platforms for payment. What is "shared business platform"? The specificity of the capital business, the existence of multiple business units, multiple technical teams, and in some cases there will be cross (B business technology team, to undertake the needs of the A business team), it is clear that the platform since the closed-loop delivery, it is very easy to form a delivery hotspot. The business technology team is close to the business, and can better prioritize. Based on this consideration, the concept of "shared business platform" was proposed at the beginning of the design of the capital business platform. Similar to a shopping mall complex, the platform team is responsible for the planning and construction of the mall infrastructure (e.g., water, electricity, coal, and regional planning), while the business team can directly lead the business of each store in terms of what kind of business it does, how it is decorated, and how it is marketed, as long as it is in line with the statute of the mall.

**2.1 Key Designs to Achieve Platformization**

**Componentized design**

! [](https://pic1.zhimg.com/80/v2-104f1a070e9764d27e7f7d6f9af51314_720w.webp)

Inspiration from Ford's automobile assembly line about how to upgrade from part-level reuse to module- and fragment-level reuse, some of our students suggested that they had previously learned that whole-vehicle architectures had similar reuse designs, and with a learning attitude we shared and studied the evolution of whole-vehicle architectures that have developed for 100 years so far, and unexpectedly came up with the key solution ideas and further confirmed the correctness of our architectural design ideas.

1) **Abstract Modules** - From the differences, compare and contrast to find the commonalities. Abstract modules as the highest common denominator of reusability of all models.

2) **Assembling modules** for business customization - Differences within the same module appear to be differences in the functions of several parts, but are essentially differences in the design of multiple parts to address various needs, such as safety, size, energy, emissions, etc. For example, each module has to understand the constraints of "safety level", and each module of the whole vehicle has different mode performance under different safety levels. To summarize the idea of platform-based vehicle manufacturing: in order to meet different personalized needs and improve assembly line production efficiency, the vehicle architecture is decomposed into a number of independent and interconnected modules in accordance with certain principles, to achieve the maximum generalization of modular parts, and to have the ability to produce models of different positioning and levels by adjusting and combining different modules. **How to realize the adaptation and assembly of components **We were surprised to find that the so-called separate lines, another completely different industries, in solving the architectural problem of the idea is so similar, through the above description, we can summarize the solution to the problem after thinking about the idea:

+ Parts abstracted into modules, higher dimensional level of abstraction.
+ The constraints to be understood by each module are abstracted into a unified interface. The modules do not depend on each other, relying on the indicators constraints interface, interface, that is, dependency inversion.

Therefore, we will reusable granularity from the original atomized java methods, up to the module level reuse granularity of components, component patterns, process phases, processes.

+ reuse granularity is neither too large, too large reuse granularity such as business processes, will lead to the inability to digest more transaction scenarios abstractions and differences, and ultimately lead to a gradual increase in business processes.
+ The reuse granularity is neither too small, too small reuse granularity such as java methods are not only unmanageable but also not very reusable.

! [](https://pic1.zhimg.com/80/v2-ac8063017878e08aa2bf461df7367350_720w.webp)

**Extensible design***

Components get abstracted and reused, but we also need to meet the customization requirements of different scenarios, the same are A->B funds transfer, but in different scenarios, the payment rules are different, such as the refund cycle, the amount of money, the payment channel, the security of the wind control and so on.

! [](https://pic2.zhimg.com/80/v2-082018b7912b627f416247ac5a8093f5_720w.webp)

We then seem to car sales manufacturers, what is the solution in response to customer customization, first of all, although many vehicle manufacturers are communal production platforms, but because of the different positioning requirements of the market, the product will be classified as a number of models of sedans, a number of models of SUVs, a number of models of MPVs. These different models themselves are different products with different "product-specific constraints" to be made in the same platform architecture. Moreover, when we enter a car 4S store, the salesgirl will often let you choose a number of "packages" for a certain model, such as the Night Edition, Sports Package, etc. After analyzing the above platform architecture thinking, it is not difficult to find out that these so-called "packages" are in fact some "specific market demand constraints" for these car models, such as under the constraints of the "sports package", the wheel parameters of the car will become bigger, the seat will be retrofitted with a sports waist protector kit, and the color parameters of the car will become a cool blue, and so on. And when you ask, can you add heated seats to the sport package, most manufacturers, probably, support these more personalized customizations.

! [](https://pic4.zhimg.com/80/v2-b30621f000c6909b396ef19cf643c913_720w.webp)

(Business Architecture Layering)

By analogy, we can also provide customers with **standard products, add-on packages**, and personalized customization based on a platform-based architecture. We can build a product layer that selects specific product features to be packaged as standard products based on the scalable and configurable capabilities of the platform layer. At the same time for the customization of the business, to provide optional "business capabilities" packages, but also based on the business application of different ** business identity **, do personalized business configuration and business code expansion. Many platforms in the industry, the business capability design of Ali e-commerce, in essence, is also through the form of layered delivery, layered governance, the formation of different levels of business reuse architecture, which also coincides with our architectural design ideas.

**2.2 Shared development and shared runtime**

Scenario extensions and platform services are isolated to realize the shared development model

! [](https://pic2.zhimg.com/80/v2-6b0959eb21fe27dc64317ef4e8e34d75_720w.webp)

After architectural decisions, we discarded the past (product layer prod) and (core layer core) divided into two applications application architecture, merged into a large platform type application, to business platform type application architecture evolution. From the front product layer decision-making extension to the scenario-based decision-making extension within the platform, we completely divorced the scenario-based extension from the platform in the form of extension packages in the code and put forward the "business containerized delivery" mode, decoupling the research and development and deployment of the scenario-based extension packages by using the serverless application architecture model. This way of decoupling business and platform through Serverless ark module package is very suitable for shared R&D team because they can't split the application and have to do business delivery on a platform, which can achieve complete isolation of platform and business in code and running state, and pure and non-polluting platform-side capability; and also can maximize the reuse efficiency of platform capability.

#### **3\. Platform capability productization design**

**The software world and the real world should be a continuous fitting process** - Through model refactoring, and platform design, the assembly of funding capabilities, can be said to be more flexible, but we realize that just for the technical flexibility is not enough, we hope that the internal capabilities of the technology platform, can be fully described and externalized into the product capabilities (product process, product functional parameters), only in this way the platform can be continuously fresh and maintain iterative. only then can the platform continue to stay fresh and remain iterative.

**3.1 Product and Technology Capability Fitting**

We all claim that we are doing business, but our code can not find these core business concepts, which leads to business and technical communication and synergy is extremely inefficient, the original workload of 1 week, basically 1 month to get off the ground, while the real code may be a few lines to get it done, this type of problem is often very common in the payment business. As shown in the figure below, in the past, we received the requirements of the model are such a generic pipeline, the ability to reuse is entirely dependent on the architect in the process of receiving the requirements of the side side of the abstract, in the end, what abstraction of the ability to PD a lot of times rely on the word of mouth. We often talk about domain-driven design - more emphasis on business domain architecture, the abstractor of the domain and the business should be closely connected, while our actual situation may not be so. Therefore, the reason why there are difficulties in feature reuse, product inheritance, and weak product operation capability is that "product capability" is inconsistent with "technical abstraction".

! [](https://pic1.zhimg.com/80/v2-018b3f05644419a9851f9ec1c53e85ec_720w.webp)

(The description of product and technical capabilities relies on experience)

**3.2 Explicit Expression of Product Capability

So, is there a way to make the product capability and technical abstraction bite more closely, or even make the product capability become the "nocode of the product function" that the product manager can write, and the code of the java written by the R&D can establish some kind of correlation? We think it is possible, we just need to do:

**1) Do delivery of technical abstract code as standardized components and component extension points. **

**2) Give the PD the Product Functionality Workbench to allow product functionality to be defined in a non-codified way. **PDs create a new funding product on the workbench and do the abstraction of the product functionality. We visualize the abstraction of the product functionality as allowing PDs to design a form form in which this functionality can be expressed, and this form can subsequently be used as an operable position for the product, which we call the operational view.

**3) Establish the connection relationship between the product function defined by PD and the abstract code of technology** (merge, association, cascade, with or, conditional, etc. relationship)

! [](https://pic4.zhimg.com/80/v2-8bd3de271d711d08b3f13c110b69b1cb_720w.webp)

(Technical specification converted to product specification)

### V. Key Architecture Design (II) - How to Make Innovation Run Faster

In the previous section, we focused on the platformization and assembly line production of capital capabilities. In this section the focus is on how we can improve the speed of business end-to-end R&D delivery to help scale the growth of funding scenario innovation. If the capital transaction ability is the core engine, then the capital scenario innovation is to connect the various components to build a complete vehicle, and quickly sell it to the market to achieve growth. The key here is to be "lightweight" and "agile". Back to payment, through the past experience of multiple product landing, we also summarized the product life cycle of innovative business, roughly divided into three stages: ** scene construction, operational growth, insight iteration **, as shown in the figure below.

! [](https://pic3.zhimg.com/80/v2-d19aeda3937747407b1635a64ba62246_720w.webp)

It is not difficult to find that scene construction, in general, requires nothing more than three major types of capabilities:

**1) scenario model-oriented construction CRUD (new)**: for example, to do an account relationship and aggregation, billing dynamics and comments, these have domain characteristics, and looking all over the station can not find reusable platform system, at this time we are required to deliver the CRUD capabilities of the new domain services.

**2) Middle-platform type domain service orchestration (reuse)**: for example, it involves accounts (shared accounts), fund-type transactions (recharge, transfer, withdrawal), social, asset core, cashier payment, security and other domain capabilities, these capabilities we obviously do not need to duplicate the construction, we only need to coordinate the ant's huge middle-platform system, the following figure our link in the fundapplication relies on the The ability of many other domains, finally we orchestrate and open the mobilegw interface, and agreed with the front-end interface protocol standards.

**3) Finally, through one front-end component after another, we build out the page and process. **Summary: the domain essence of funding scenario innovation = the construction of the scenario domain CRUD (new) + orchestration and aggregation of existing domain capabilities

#### **1\. Lightweight build and growth system**

There is a part of the scene construction, are lightweight pages and processes, to cite a few of our CY21 years of innovation business common cases, such as ants together flowers (small wallet) product account opening pull new scene, transfer living expenses scene, C / B of the new balance of small program positions.

! [](https://pic2.zhimg.com/80/v2-35570d233fc2b13025574fbb8d18465d_720w.webp)

These partial fixed positions, marketing positions of product development, basically divided into the front-end small program interaction page (front-end) + content services (server-side) + simple functional interaction services (server-side), in addition to the product and operation students may mention some of these positions for the refinement of the operational requirements as well as the operation of the post-operational data analysis requirements. **One-stop building and growth platform** In CY21, we started to build a low-code building platform: a one-stop operation platform with various capabilities such as refined operation capability, fast building, data intelligence, and multi-opening, etc., which will improve the efficiency of R&D and operation. **1) Rapid construction: **For the field/position/product page Provide a one-stop solution for rapid low-code construction, enhance the R&D efficiency of front-end and back-end students. -Essentially the pattern is relatively fixed in this field, the traditional manual service-oriented aggregation mode upgraded to the front and back-end integration of the template way to do product upgrades. **2) refined operation:** can be business self-service one-stop page/module based thousands of people face refined operation capabilities, greatly improving the efficiency of business operations. --Templating must be able to nocode way evolution, nocode will certainly bring changes in production relations, this part of the work from the technical people into the business self-service, can be said to be a win-win situation. **3) Data Intelligence:** complete all-link data standards and standard ride cast data effect products, through the unification of buried points, off-line data insight system, injected into the operational process of the operational strategy and business indicators, to meet the operational students in the operational process of the data insight needs, to enhance the transformation of business growth. --After unified templatization, the data is easily unified.