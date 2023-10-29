---
title: Seller's and Buyer's Shows of Database Performance
description: Seller's and Buyer's Shows of Database Performance
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

> Article source: WeChat public number "All brothers within the four seas"
> 
> Author: Xue Xiaogang, Oracle ACE/PG ACE partner/ TiDB MVA, Evangelist / OCP Lecturer / ITPUB Core Expert / InkTen Wheel MVP / Oracle Certified / MySQL Certified / PG Certified / Oceanbase Certified / Damon Database Certified / TiDB Certified




Last week, I met a friend from Huawei who mentioned a lot of performance metrics when it comes to databases. First of all, I do not doubt his indicators, I think nine times out of ten is true. Now as long as the data, basically reliable. Because if outrageous data, peers will question. But I said a little bit is that in the actual environment, usually users can not even reach this one percent. It's not that the product doesn't work, it's that the users don't work.




We database practitioners debate database performance to see which metrics, centralized or distributed. (Yesterday, I read an article about the comparison of distributed and centralized TPS and QPS, etc.) As for whether to look at TPS, QPS or RT, it is difficult to convince the other side for a while. And I just said that in the actual environment in fact, no matter which indicators to look at, are not reached. This is because the official data are ideal data in the laboratory, while the actual environment is very harsh. The status quo in our country is that application developers do not understand the database, and I do what the requirements say. In fact, the logic of the requirements is all wrong, and he doesn't care. Coupled with the database does not have a good design, SQL thousands of tens of thousands of lines, and even a SQL hundreds of MB. These poisonous beatings from the user's actual use, resulting in the database what performance is being held down on the ground friction.




Once upon a time, POC may be a few hundred pieces of data, now we have been fooled more, have a heart. There may be measured when the data volume of 10 million level or even hundreds of millions of data volume. Once saw a few domestic database vendors complained that the user test their products require 10 million tables do not build indexes to test performance. Vendors feel aggrieved. But in fact this is our domestic status quo. Because most of the business scenarios that use Oracle, there is no index. This is the status quo in China. To put it bluntly, only the function, performance is not his assessment indicators. As long as the functional logic on the line. However, in my handling of some of the problems of various enterprises found. Sometimes the functional logic is not right.




So the user knows what their status quo is, very little control over the quality of development. Then the A database on the writing can resist, then change the B database do not expect application development to change the program. The same messy writing depends on whether it can be supported. Can not support it will not be considered.




This is what I mean by the database performance of the seller's show (think good designers, development is a high level), while in fact is only the vendor can not think of, no developers can not do. Descartes product understood? Needs to experience a bit of a reality hit.




We see TPCC data amazing, that is not seen our real environment of table design, requirements and SQL. with this situation to set TPCC, not to mention the hit list. If it is not down or not down, it is still a matter of opinion.




A friend from Huawei said that he wanted me to provide a real user scenario to see how their products can be prevented. I saw a different Huawei product attitude. They are not the ones who say they are far ahead. I appreciate their attitude. I have nothing else, there are too many blood and tears lessons in this area, and these are not common in Internet companies, which is related to the company's genes.




Based on the above conclusions, the performance problems caused by scribbling are also problems on distributed databases such as OB and TiDB, and this has nothing to do with standalone or distributed. Dili hot bar wearing a good-looking clothes, a 200 pounds to wear, not to mention good-looking, clothes will not OOM be held open are two say, this and a separate top or dress has nothing to do. The main thing is that the status quo is that everyone's figure management is not in place.




It is possible that only by taking it seriously and managing it properly can we reduce and avoid these kinds of problems, for example, drunk driving is almost unheard of nowadays. When it comes to such times, talk about how to test database performance and distributed and centralized performance.