---
title: There's no myth about the big players when it comes to streaming computation.
description: There's no myth about the big players when it comes to streaming computation.
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

Jitterbug and Today's Headlines, the two most popular products under ByteDance, are also the facade of ByteDance. Behind this, there are many technical teams in support, and streaming computing is one of them.



However, even in ByteDance, there is no myth to engage in streaming computing. Only a group of young people, spent six years, one step at a time, from the beginning of the "do not know the technology does not know the business", and finally carried the byte internal streaming computing platform and the construction of application scenarios, supporting the machine learning platform, recommendation, number of warehouses, search, advertising, streaming media, security and wind control, and other core business. In 2022, the team completed the cloud biochemical transformation of the Flink computing engine and formally provided cloud capabilities to the public through the volcano engine.



This is not a heroic story that saves the day, there are no ups and downs, and there are no dazzling flowers and applause. Instead, it's a record of a small group of millions of ordinary developers, passively accepting growth in business while actively seeking breakthroughs in open source.



### 01 **Code to be written and business to be pulled***



In 2019, with the outbreak of Jitterbug, ByteDance stood at the beginning of high speed growth, and live broadcasting, short video, advertising and other businesses all rode the wave. These businesses, all of which need streaming computing to support them.



** Zhang Guanghui, the person in charge of the byte streaming computing team, is facing many thorny problems. **



First put the timeline forward two years, when Zhang Guanghui just joined the byte jump, the calculation engine with Apache Storm - born in 2011, Twitter developed the first generation of stream processing system, only support some low level API.



"All Storm tasks are submitted in scripts on the development machine, and the operation and maintenance platform is in a very primitive state. If the Storm cluster fails, the jobs can't be automatically recovered, and you can't even find all the stock jobs." Zhang Guanghui remembers this vividly.



That said, no one is too proud of anyone. At that time, Zhang Guanghui's resume, there is no streaming computing product experience, but some "relatives" - involved in streaming computing upstream and downstream product development, such as data collection, message queuing.



The good thing is that byte's business scenario is single, mainly focusing on machine learning scenarios, Zhang Guanghui and his team switched the streaming computing engine from Apache Storm to Apache Flink. the so-called team, in fact, even including him, there are only two people. Then in 2018, he worked with the data flow team to complete the construction of the streaming computing platform, including the monitoring and alarming of tasks, log collection, anomaly diagnosis and other tool systems.



In 2019, the business scenarios to be supported by streaming computing have been quite rich, expanding to real-time number of warehouses, security and wind control, and so on, and are still increasing. The demand for individual scenarios has also become more complex: the recommendation business is getting bigger and bigger, with a single job exceeding 50,000 Cores; the real-time warehouse business scenario requires SQL for development and has higher requirements for data accuracy.



However, due to the serious shortage of manpower in the team, the work progress was very slow. "There are only two people on the team, and Oncall takes turns to be on call. When they weren't on duty, they were often solving problems left over from the previous week's Oncall." Zhang Guanghui described it this way.



Zhang had to expand his staff while working with the data integration team to build the SQL platform. It was at this time that Benchao Li joined the Streaming Compute team and, shortly after, became the technical lead for Flink's SQL direction.



**However, using ** **SQL** ** to develop **** streaming computing **** tasks ****, Benchao Li didn't have much experience: "At the beginning, I didn't understand the technology, nor the business." **



Prior to this, he served in a small and medium-sized enterprises, the scope of work involves a wide range of streaming computing can only be counted as one of the directions. After joining Byte, Li Benchao realized that the scale of Byte's streaming computing far exceeded his imagination. Before you can only see a concurrent task, but in the byte, the concurrency of a task can be tens of thousands, only a single task to use the computing resources than the previous company all the tasks are added up.



But Li Benchao can not help but understand. Five days a week at work, three of them, Zhang Guanghui first thing in the morning caught him and asked, with which business chat, can create a few new SQL tasks.



** indicators every day in the head spinning, Li Benchao had to give the team "pull business". **The use of words is the same as in the street to stop passers-by to sell products, but the location changed to bytes in Beijing's various work zones.



"Hey, we can develop this streaming computing via SQL, are you interested? Do you want to know more about it?" Li Benchao has nothing to do but contact the heads of e-commerce, live broadcasting, advertising, games, education and other business departments. As long as people nodded, Li Benchao immediately took the shuttle bus and went to the work area for on-site exchanges without saying a word.



Zhang Guanghui commented, "At that time, it was really 'doing everything'."



With the SQL platform, the development and maintenance efficiency soared. "Originally, it took one or two days for one person to develop a task. And now, one person can directly handle ten tasks in one day. In addition, the way of communication between the business side and us is much simpler, and we can understand the code written by the other side, which makes it easy to optimize."



In addition, Byte has done a lot of work on Flink's stability, such as supporting the blacklist mechanism, single point of failure recovery, Gang scheduling, speculative execution, and other features. Since the business has higher requirements for data accuracy, the team supports the operation of the Checkpoint mechanism to ensure that the data is not lost, and has been widely promoted and implemented in Byte.



In this process, Li Benchao also found that Flink may not be as powerful and easy to use as he thought, for example, it is not compatible with a random change of SQL status. In order to address these problems that have not yet been solved by the community, Byte has also carried out a lot of internal optimization solutions to explore.



! [](https://oscimg.oschina.net/oscnet/up-e8d825d23950c5e2d58dc9a101db0d82ab1.png)



**ByteHopping* *Flink* *SQL* *TaskConcentration



### 02 **Flink **** turns out to be more than streaming computing**



After ByteDance chose Flink as its streaming computing processing engine, tens of thousands of Flink jobs run on its internal cluster every day, with peak traffic as high as 10 billion data items per second. The scale of individual jobs is also very large, each compute node uses about 30,000 concurrency, and the whole job uses more than 300 physical machines. the stability and performance optimization of the Flink cluster, as well as the optimization of the deployment, execution, and failure of a single very large job, face problems that are hard to find a second in the whole industry.



Since Flink is a streaming and batching computing engine, ByteDance has actively promoted Flink's streaming and batching implementation, and has launched more than 20,000 Flink batch jobs, in which it has solved a lot of stability and performance problems, such as Hive syntax compatibility, slow nodes, and speculative execution.



At the same time, Byte Jump has launched the ByteHTAP project internally. Combined with Byte's internal OLTP system, Byte has been able to support analytical calculations with low data latency (sub-second) and high data consistency requirements, but there is still a lack of a compute engine to support OLAP calculations. As Byte has done a lot of in-depth optimization in Flink, it is finally used as the OLAP engine of ByteHTAP.



! [](https://oscimg.oschina.net/oscnet/up-0bdaccfa52987511e6780144f12d4450ee4.png)



**However, **** as ByteHTAP** **began to provide online** **OLAP** **services to the business side, a new problem arose. **Not only did the business require latency for a single concurrent query, but they also wanted the team to provide an OLAP service that could support high concurrency.



At the beginning of 2021, Fang Yong joined ByteDance as a streaming computing architect. In order to support the online business, Fang Yong and his team had to make up for this capability as soon as possible.



"The whole development process was very torturous and stressful." Fang Yong said, "ByteHTAP has already provided online services, we need to iterate quickly to make Flink support higher concurrent queries."



Every time the team had a weekly meeting, Fang Yong would keep an eye on the QPS metrics. It took nearly half a year to "finally optimize QPS from single-digit to dozens and dozens, until a single cluster on the line supports hundreds of QPS."



In the last two years, Byte is contributing many of the Flink OLAP optimizations back to the community. Flink OLAP content has also been added to the Apache Flink 2.0 Roadmap.



A complete data production chain is divided into three computing scenarios, namely streaming, batch and OLAP computing. In the real-time warehouse scenario, Storm or Flink is needed to support streaming computation; in the batch scenario, Hive or Spark is relied on, and when the computation semantics are different, the two sets of engines will lead to inconsistencies between streaming and batch results. Moreover, after streaming and batching data, it needs to be imported into the warehouse or stored offline, and then a new OLAP engine has to be introduced to probe and analyze the data, which can't guarantee the correctness and consistency even more.



Moreover, optimization and maintenance is also quite troublesome. Three systems mean that three teams have to be built to maintain them separately. Once there is a need to optimize or solve bugs, it is necessary to raise issues to the three communities for discussion.



The Flink community proposed Streaming Warehouse to solve this problem. Bytes investigated the current development direction of streaming computing and the Streaming Warehouse system, and built the Streaming Warehouse system based on Flink and Paimon, which unified the computation and storage of streaming and batch, and increased job and data lineage management, data consistency management, and data management. We added core functions such as job and data lineage management, data consistency management, streaming data revision and backtracking, etc. to solve the problems of streaming computing accuracy and data operation and maintenance.



! [](https://oscimg.oschina.net/oscnet/up-fca6618852b7122974aca81d2377233a202.png)



**In the end, "three engines, three teams" became "one engine, one team". **In Fang Yong's words, using Flink as a unified streaming, batch and OLAP computing engine for the entire data production chain, there is no need to worry about the real-time data and the complexity of business analysis.



As for the future of Flink, Fang Yong already has a vision. He hopes to gather the R&D capabilities of the community to improve the entire Flink computing ecosystem, and turn Flink into a Streaming Warehouse system that unifies streaming, batch and OLAP.



### 03 New Business, New Scenarios, New Challenges



In 2022, "Streaming Flink Edition", a commercialized computing engine developed by Byte's Streaming Computing team, will be launched on the Volcano Engine, officially providing computing power on the cloud to the outside world instead of only serving Byte's internal business.



In Byte, this product is called "Serverless Flink", which relies on ByteDance's largest real-time computing cluster practice in the industry, and is based on the Volcano Engine Container Service (VKE/VCI) to provide Serverless extreme elasticity, which is a new generation of out-of-the-box cloud primitives. Out-of-the-box, next-generation cloud-native, fully managed real-time computing platform.



**In fact, it may not be appropriate to call ** **Serverless** **Flink** **a **** newly launched product ****. **Li Benchao explained that the so-called "streaming computing flink version" is actually the team in six years, so that Apache Flink in the byte internal realization of large-scale application, and the accumulation of a large number of product experience and technical capabilities "packaging" a little bit, rather than the accumulated product experience and technical capabilities. "Instead of making a new product, it is based on a derivative of Apache Flink.



It is derived from Apache Flink, can be understood as an enhanced version of Apache Flink, and is 100% compatible with Apache Flink, including many features:



+ development efficiency. Streaming computing Flink version supports operator-level Debug output, Queryable State, Temporal Table Function DDL, which significantly improves the development efficiency of the open source version of Flink.
    
+ Reliability improvement. Streaming computing Flink version of checkpointing for a single task, improving the success rate of checkpointing under high concurrency. Single-point task recovery and node blacklisting mechanism ensure fast response to faulty nodes and avoid overall business restart.
    
+ Serverless cloud-native architecture. Extreme elasticity, 1‰ core fine scheduling.
    
+ Ease of use enhancement. Minimal SQL development, out-of-the-box, O&M-free, supports full lifecycle management of streaming data.
    
+ High performance at a low price. Cost-effective, SLA-guaranteed, ultra-low TCO.
    



High performance and low price. [](https://oscimg.oschina.net/oscnet/up-94e4c01fd23fdb7e7327a975d5f2575b6dd.png)



**Streaming Computing** **Flink** Version Architecture Diagram*



**After Serverless** **Flink** ** went live with the Volcano engine, Fang Yong realized that external customer needs were very different from internal business needs. **For example, some customers are still using relatively early streaming technology stacks such as Storm and Samza. Therefore, the team not only needs to provide technical training and support to customers, but also to help the