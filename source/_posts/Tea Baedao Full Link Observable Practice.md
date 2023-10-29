---
title: From climbing to running - why do we need unit tests?
description: From climbing to running - why do we need unit tests?
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

Cha Ba Dao is a local tea beverage chain brand in Chengdu, Sichuan Province, founded in 2008. After 15 years of development, Cha Ba Dao has become a benchmark brand of food and beverage, with more than 7,000 stores in 31 provinces and cities across the country, realizing the full coverage of all provinces and line-level cities in mainland China. on March 31, 2021, in the Chengdu-Chongqing Food and Beverage Summit, Cha Ba Dao was awarded the "2021 Chengdu-Chongqing Food and Beverage Benchmarking Brand Award". In August 2021, Cha Baidao was selected as one of the "Top 15 New Tea Drink Brands in China in the First Half of 2021" by iiMedia Ranking.2023 On June 9, Cha Baidao, a new tea drink brand, secured a new round of financing led by Soroptimist Asia and followed by a number of renowned investment institutions, increasing its valuation to 180%, which is the highest in China. The valuation soared to 18 billion yuan.



In April this year, Cha Baidao held a brand upgrade conference at its headquarters in Chengdu, announcing that the number of stores had exceeded 7,000. According to the China Chain Store Management Association, by December 31, 2020, 2021 and 2022, the number of Tea Budao stores will be 2,240, 5,070 and 6,532, respectively, and the epidemic has not slowed down the pace of its expansion.



**With the rapid expansion of its business scale, Cha Ba Dao accelerated its digital transformation strategy. **But as some of Tea Baidao's early business systems were provided by external SaaS service providers, they were unable to meet the requirements of large scale, high concurrency, elasticity of expansion, agility and observability brought about by the rapid growth of online business. In order to meet the needs of online and offline store customers and business growth, for store services, POS, user transactions, platform docking, store management, food and beverage production and other core chain services, **TeaBaido chose to comprehensively self-research combined with the native capabilities of Aliyun Cloud to promote containerization, microservicing, and observable capabilities for a comprehensive upgrade. **



### Business value of cloud nativeization



The tea beverage industry faces the pressure of market competition and the need to improve internal operational efficiency. To meet these challenges, AliCloud and TeaBaido work together to complete the transformation of Cloud Native to the cloud and start a new journey of digitization.



The use of container and microservice technology realizes the lightweight and high portability of applications. It allows enterprises to deploy and expand applications more flexibly and respond quickly to market demand, enabling them to realize high availability and elastic scalability of applications, and maintain stable business operation regardless of whether they face sudden peak access or system failures. The introduction of continuous delivery and continuous integration of development methods helps enterprises realize rapid iteration and deployment. By automating the process, enterprises are able to roll out new features and products faster, keeping pace with the market and grabbing a head start.



The cloud-native transformation to the cloud not only brings higher security, availability and scalability, but also improves an organization's ability to innovate and be competitive.



### Observable challenges brought by cloud native



As an emerging restaurant brand with rapid business development, TeaBaido has a huge number of online orders every day, which is backed up by a close integration with Internet technology, with the help of extremely high digital construction to support TeaBaido's huge sales volume. Therefore, there are very strict requirements for the continuity and availability of the business system to ensure the stable operation of the core services of the transaction chain. Especially during daily peak ordering hours, marketing activities, and unexpected hot events, in order to provide users with a smooth experience, each link of the entire microservice system needs to ensure the quality of service under high concurrency and high traffic.



A perfect all-link observable platform and APM (Application Performance Management) tools are the prerequisites for guaranteeing business continuity and availability. In the construction of observable technology system, TeaBudo technical team has experienced more exploration. Before the full realization of containerization, TeaBaido accessed the open-source APM tool on some microservice systems and carried out more than a year of validation, but in the end, it was not able to be promoted to the entire microservice architecture, mainly due to these reasons:



+ **The balance between metrics data accuracy and sampling rate is difficult to trade-off**.



Appropriate sampling strategy is an important means to solve the cost and performance of link tracking tools. If the APM tool is fixed to use 100% link full collection, it will bring a large number of duplicate link information to be saved. Under the huge microservice system scale of Chabaidao, 100% link collection will cause the observable platform storage cost to exceed expectations, and it will also have a certain impact on the performance of the microservice application itself during the peak business period. However, open-source tools can affect the accuracy of the indicator data in the case of setting sampling strategies, so that important observable indicators such as error rate, P99 response time and other important observable indicators will lose the value of observation and alerts.



+ **Lack of higher-order alerting capabilities



Open-source tools are relatively simple to implement in terms of alerts, and users need to build their own alert processing and alert distribution platforms in order to realize basic functions such as sending alert information to IM groups. Due to the many service modules and complex dependencies after the microservicing of TeaBaidu, it is common for a component to be abnormal or unavailable. Often, the abnormality or unavailability of a component leads to a large number of redundant alarms in the whole chain, forming an alarm storm. The result is that the operation and maintenance team is tired of dealing with a variety and huge number of alarm messages, and it is very easy to miss the important messages that are really used for troubleshooting.



+ **There is no single means of troubleshooting**.



Open source APM tools are mainly based on Trace link information to help users realize fault location, for simple microservice system performance problems, users can quickly find the performance bottleneck or fault source. However, in the actual production environment, there is no way to solve many difficult problems through simple link analysis, such as N+1 problems, memory OOM, CPU occupancy is too high, the thread pool is full and so on. This puts high demands on the technical team, which needs engineers with in-depth understanding of the underlying technical details and rich SRE experience to quickly and accurately locate the root cause of the failure.



### Accessing AliCloud's application real-time monitoring service ARMS



In the process of comprehensive cloud biochemistry of TeaBaido's system architecture, TeaBaido's technical team and Aliyun's engineers discussed in depth the better way of landing on the ground for full-link observability.



As an important member of AliCloud's cloud-native observable product family, ARMS application monitoring provides thread profiling, intelligent insights, CPU & memory diagnostics, alarm integration, and other capabilities not available in open source APM products. At the suggestion of AliCloud, TeaBaidu technical team tried to connect a business module to ARMS application monitoring.



Since ARMS provides automatic application access in container service ACK environment, it only needs to add 2 lines of code to the YAML file of each application to automatically inject probes and complete the whole access process. After a period of trial, the practical value provided by ARMS application monitoring has been continuously explored by Chabaidao's engineers. TeaBaido also uses AliCloud's performance testing product, PTS, to realize the capacity planning of daily routine and big promotion. Because of the introduction of ARMS and PTS, TeaBaido's daily operation and maintenance and stability assurance system has undergone many upgrades.



### Build emergency response system around ARMS alert platform



Since the problem of alarm storms was often encountered when building an alarm platform based on open-source products, Cha Budao is very cautious about the configuration of alarm rules, and converges the alarm targets to the most serious business failures as much as possible. Although this can avoid the frequent harassment of the SRE team by the alarm storms, a lot of valuable information, such as a sudden increase in the response time of the interfaces, can be ignored.



In fact, for the problem of alarm storms, the industry has a set of standard solutions, which involves de-emphasis, compression, noise reduction, silence and other key technologies, but these technologies and observable product integration on the existence of a certain degree of complexity, a lot of open-source products do not provide a perfect solution in this area.



These key technologies in the field of alarms, in the ARMS alarm platform have a complete function. Take event compression as an example, ARMS provides two types of compression: label-based compression and time-based compression. Multiple events that meet the conditions will be automatically compressed into a single alert for notification (as shown in the figure below).



! [](https://pic2.zhimg.com/80/v2-f8f820cb1a1e2e4cb9e31892a88154ed_720w.webp)



Figure: Tag-based compression



! [](https://pic3.zhimg.com/80/v2-63455c4e67cec8ccd99354982ec160ae_720w.webp)



Figure: Time-based compression



With the various technical means provided by ARMS alert platform, the problem of alert storm can be solved very effectively. Therefore, the technical team of TeaBaiDao began to pay attention to the use of alerts, and gradually enriched more alert rules, covering different levels such as application interfaces, host metrics, JVM parameters, database access, and so on.



Docking through the enterprise WeChat group, so that the alarm notification to realize the interaction of the ISTM process, when the duty officer receives the alarm notification, you can directly through the IM tool for alarm closure, event escalation and other capabilities, to quickly realize the alarm processing. (as shown in the following figure)



! [](https://pic2.zhimg.com/80/v2-868ec5e58d249070b4136ded69da9f51_720w.webp)



Figure: Intelligent convergence and notification of monitoring alarm events



The flexible and open alarm event disposal strategy meets the needs of different timeframes and scenarios. TeaBaidu started to build an enterprise-level emergency response system based on this with reference to Alibaba's best practices for security production. The emergency scenarios from the business perspective are used as the core model for event emergency response, and the corresponding fault handling process is identified and flowed through different alarm levels. These are the experiences that TeaBaidu has worked out after the full-scale cloud-native biochemistry, and significantly improve the quality of service in the production environment.



### Introducing Sampling Strategy



Extracting metrics data from link information is a necessary function of all APM tools. Unlike the simple and crude way of extracting metrics in open source products, ARMS application monitoring uses end-side pre-aggregation to capture every real request, first aggregating, then sampling, and then reporting to provide accurate metrics monitoring. It ensures that the metrics data remains consistent with the real situation even if the sampling policy is enabled.



! [](https://pic1.zhimg.com/80/v2-1dcc3e8146be2d84b8fb7d7e6494f9bc_720w.webp)



Figure: ARMS end-side pre-aggregation capability



In order to reduce the application performance loss caused by APM tools, TeaBaido adopts a 10% sampling rate for most applications and an adaptive sampling strategy for applications with very high TPS to further reduce the application performance loss during peak hours. **Through real-world testing, during peak business hours, the application performance loss caused by ARMS application monitoring is more than 30% lower than that of open-source products, and the accuracy of metrics data can be trusted, **such as the average response time at the interface level, the number of errors, and other metrics can meet the needs of production-grade business.



The application performance loss caused by application monitoring is more than 30% lower than that of open source products. [](https://pic4.zhimg.com/80/v2-ba2517b5cb0ae39b775735ae29c347fb_720w.webp)



Figure: Interface level metrics data



### Asynchronous links are automatically buried



Asynchronous thread pooling technology exists in the Java space, as well as numerous open-source asynchronous frameworks, such as RxJava, Reactor Netty, Vert.x, and others. Compared to synchronous links, asynchronous links are more technically difficult to automatically bury points and context pass-through. Open source products have incomplete coverage of mainstream asynchronous frameworks, and there is the problem of burial failure in specific scenarios. Once such a problem occurs, the most important link analysis capability of the APM tool will be difficult to play a role.



In this case, developers need to manually bury points through the SDK to ensure the context transmission of asynchronous links. This creates a huge amount of workload and is difficult to be quickly rolled out in a large scale within a team.



ARMS supports all major asynchronous frameworks, enabling asynchronous link context transmission without any business code intrusion. Even if some asynchronous frameworks are not supported in a specific version in a timely manner, as long as the user side puts forward the requirements, the ARMS team will be able to make up for it in the new version of the probes. **After using ARMS application monitoring, the TeaBaidu technical team directly cleaned up the manual buried code of the previous asynchronous frameworks, significantly reducing the maintenance workload. **



! [](https://pic2.zhimg.com/80/v2-7a53aa7ffcc415d88ab3ffdbaf21a219_720w.webp)



Figure: Link context of an asynchronous call



### Utilization of higher-order application diagnostic techniques



When the coverage of buried points is high enough, traditional APM tools and link tracing tools can help users quickly determine which link (Span) has a performance bottleneck, but they cannot provide more effective help when they need to investigate the root cause of the problem further.



For example, when the system CPU utilization rate increases significantly, is it caused by a business method consuming CPU resources like crazy? This is a difficult problem for most APM products to solve. Because it is impossible to know the resource consumption of each link from the link view alone. TeaBaidu engineers have encountered similar problems many times when using open source tools. At that time, they could only make guesses based on experience, and then go to the test environment to make repeated comparisons to solve the problem completely, although they also tried some Profiling tools, but the threshold of using them was relatively high, and the effect was not very good.



ARMS Application Monitor provides CPU & Memory Diagnostics, which can effectively find bottlenecks in Java programs caused by CPU, memory and I/O, and break down the statistics by method name, class name and line number, and ultimately assist developers in optimizing the program, reducing latency, increasing throughput and saving costs. CPU & Memory Diagnostics can be turned on temporarily when a specific problem needs to be troubleshooted, and help users directly find the root cause of the problem through the flame diagram. In a scenario where the CPU of an application in the production environment spiked, Chabadao's engineers were able to locate that the problem was caused by a specific business algorithm through CPU & Memory Diagnostics.



! [](https://pic3.zhimg.com/80/v2-04ca60e7bb473058c97e5d4e