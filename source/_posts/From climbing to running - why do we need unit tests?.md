---
title: From climbing to running - why do we need unit tests?
description: From climbing to running - why do we need unit tests?
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

### ♪## Ex-language



**Does braking reduce or increase speed? **We usually think of writing single tests as a power move, delaying development progress, as if we are "putting the brakes on" the project. Massive Home Shapes takes this question and looks down the road to talk more about why unit testing can make software development run faster.



#### What is unit testing



Mass home for a single test should not be unfamiliar, intercept a Wikipedia definition to help the masses to wake up a memory:



In programming, a Unit Test, also known as a Module Test, is a testing exercise that performs computer correctness checks against program modules (**the smallest unit of software design**).



The idea of unit testing has always been part of Smashing. When we write a computer program for the third time, we will definitely output some sample data outputs and intersperse a huge amount of System.out.println in the code to make sure that each sub-node meets the expectation. This process is actually the process of breaking down a complex problem into sub-problems and breaking them down one by one. The purpose of unit testing is the same, to **guarantee the correctness of the smallest unit of a software program, so that the correctness of the complex system built from that smallest unit can be guaranteed. **.



To get a deeper understanding of the need for unit testing, let's take a look at how testing systems have evolved before going to Kauai.



#### Test System Evolution



! [](https://pic3.zhimg.com/80/v2-4927197f57105727cacf2d54360c60de_720w.webp)



Software testing has even been a stereotypical job (QA, tester) in the past, where the daily task of the QA/tester was often to perform the largest number of manual tests, tedious and error-prone.



Automated testing in the software industry has changed dramatically since the early 2000s. To cope with the size and complexity of modern software systems, developer-driven automated testing practices have evolved. It is possible to get rid of the tedium of manual testing and utilize software to test software. However, the practices of the past still leave their inevitable impact, software testing is still a homogenous workforce, the QA of the past has evolved into the SDET (Software Development Engineer in Test)), and although we have evolved to be able to use tools, we are still just monkeys who know how to use them. Why? Because this model of R&D/test separation itself leaves a lot of problems.



**When R&D and testing are two positions, the boundary of delivery is the overall functionality (functional requirements) and usability of the software. R&D only needs to ensure that the software as a whole is functionally complete and usable, and testing also focuses on integration testing and end-to-end testing. However, software is composed of countless small units, and under this system, people will focus on the quality of the smallest unit, whether it is the only one that can be tested and evolved, which will inevitably result in the "ultimate in the end, but in the middle of the failure".



Based on the various shortcomings, Song, Microsoft and other companies that attach great importance to the quality of R & D are transitioning from the 2.0 era of **SDET to the 3.0 era of integration: **Microsoft removed SDET in 2015, and took the lead in proposing the concept of "Combined Engineering" in the Bing transformed by Luqi; Song also replaced SETI with EngProd (Engineering), which is the first of its kind. The concept of "combined engineering" was first introduced in Bing, which was revamped by Luqi; SETI was also replaced by EngProd (Engineering Productivity), which specializes in the construction of test platforms and tools, and is not responsible for the specific business logic testing.



### Why unit testing is needed



In today's Internet era, the speed of software iteration is getting faster and faster, and the responsibilities of R&D are also getting more and more, and the concept of DevOps is "you build it, you run it", and the trend of R&D/testing being a two-dimensional unit can be interpreted as the trend of "**you build it, you test it**". You build it, you test it**". When R&D is responsible for the quality and testing of automated code, good testing practices are essential.



#### The Tower of Testing



Just as a building is built from the foundation up, waterproofed and flooded, testing has a similar testing pyramid structure. The diagram below from the Testing chapter of Software Engineering at Google summarizes Google's best practices for one-day testing. We can see that the testing pyramid consists of three layers. The bottom layer is unit testing, which accounts for 80% of the total weight and is the foundation of a software system. Further up are integration testing and final testing, which account for 15% and 5% respectively. Because of the decreasing contribution from the bottom to the top, it is called the Tower of Testing (similar to the construction of high-rise buildings). This ratio is recommended by Kenge as a result of many years of practice, and is intended to improve the efficiency of R&D (productivity) and enhance product faithfulness (product confidence).



One of the core concepts of the Pyramid Tower is **Unit Test First**, where the first test item in every software project should be a single test** (TDD even believes that the first piece of code should be a single test), and the highest-weighted test in the only project should also be a single test.



[]() [](https://pic3.zhimg.com/80/v2-db2743517a9f5efe25f291b209eaf40a_720w.webp)



#### Good Software Unit Testing



What is the importance of unit testing in the industry? "Isn't it better to write only end-to-end tests? That's why here we'll expand on the benefits of unit testing.



**Enhanced Debugging Efficiency



Unit tests are an excellent foundation for software processes because they are fast, stable, and dramatically compress the scope of the problem, increasing the efficiency of troubleshooting.



+ **Tests are faster:** Unit tests have no other external dependencies and run fast, providing a faster feedback loop to find and fix problems faster.
+ **Testing is more stable:** Again, because of the 0 dependency, single testing is more stable than other types of testing, and will not be affected by incompatible changes to other external modules. Therefore, single test is also the type of test that can lead to developers' carefulness**. **
+ **Problems are easier to locate:** Single tests are bounded by the largest small software unit, so growth problems can be narrowed down for localization. By contrast, the higher up the pyramid of test types, the more difficult it is to locate problems. Complex end-to-end testing involves groups of modules that require consistency queues to check and locate problems.



**Improving Code Quality



Code is written for others to see, good code should be easy to read, easy to change, easy to maintain. **The process of writing a single test is actually the process of eating autonomy and its own code dogfood (dogfood), and using autonomy and its own code **from the user/R&D vista **helps us to improve the quality of the code.



+ **Good code is easy to measure:** The concept of Cyclomatic Complexity was introduced early in the market to simplify the complexity of a module's alternative structure by quantifying the number of independent paths to walk, which can also be interpreted as the number of test cases that cover all possible scenarios at least for the user. Circle complexity is a great indication of the complexity of the judgment logic of the program code, which may be of low quality and difficult to test and maintain. Therefore, the code is good for a thing must be low circle complexity, but also easy to test.
+ **Modify Iterative Evolution:** No software is unchanging, a good software system should be easy to evolve. Single tests cover more all the project modules are more original, have clearer boundaries, and are easier to get up. The risk of rebuilding a project with a higher single-test coverage is also relatively small, exactly ** a complex project without a single-test coverage is something no one dares to touch. **
+ **Better design:** As mentioned earlier, a good single test can improve the quality of the code. If a certain R&D needs to write a single test for his own code, he is concerned about the hierarchical division of the code, reducing the excessively long, circle complexity to achieve a higher method. The following example is the cognitive complexity value of a piece of code without a single test (which can be understood as a modified version of the circle complexity, simple from the point of view of whether the code is easy to understand or not), exceeding the standard by a factor of 3 to a large extent. Now that I look back and try to fix the single test, my brain is maxed out.



! [](https://pic1.zhimg.com/80/v2-58e5b118d505a32d0bd0369a6451b5b4_720w.webp)



**Improve overall R&D efficiency***



**Single testing that improves quality and perfects speed can improve the quality and efficiency of R&D and speed up the overall delivery of the project goal. This statement is counterintuitive at first glance, as writing a single test is often more important than writing implementation logic**, which is also the most common reason for most companies not to write a single test: "the project is in a hurry, so it's too late to write a single test". If our project's ecological life cycle is calculated in months, write a prototype soon after the line, then write a single test does not return on investment does not improve. **But Ari has a lot of to B business, providing users with the power of the life cycle are calculated in years, improve the quality of the code over time, the return on investment will be increasingly improved **, specifically in the following areas:



+ **Reduced debugging time:** The reasons for improved debugging efficiency mentioned above were mentioned above and will not be repeated here. The time spent on debugging can be saved by having a higher single-test coverage, and the number of bugs in the project itself will be lower when there is sufficient test coverage. Let's take a real-life example: a team, due to historical debts, basically relies on final tests and has no unit test coverage. The consequence is very serious, the team oncall students > 50% of the time are fixing all kinds of strange bugs, can not invest valuable strength to architectural upgrades and other long-term more important projects.
+ **Increased awareness of code changes:** As I mentioned earlier, no one dares to touch code without test coverage, **Code with sufficient single-test coverage can significantly increase **Awareness and desire to remodel the code. To give you another example: I got my ADR almost ten years ago when I worked at Google headquarters. If you've ever worked at Google, you've noticed that your code often receives code changes initiated by unrelated team members. In the vast majority of cases these are mass refactors developed by fellow students, such as helping to refactor your Java code if it doesn't use the Builder pattern (these refactorings are simplified by the mass automation tools available at Google). We desperately want to leave aside the fact that if the code is not covered, or if there is no relevant group, would you dare to refactor it that way?  **
+ **Enhance the autonomy of code:** Text files can enhance the autonomy of code and make development more efficient. A good single test can actually be elevated to the text file of the code, by reading the test can quickly understand the use of the code (cf.) ⻅ TDD). Single test as a text file at the same time also perfectly solves the problem of text file freshness, giving developers a set of high-grade quality, with the code constantly updated text file.
+ **More efficient code review:** Not all problems and design flaws are found through static inspection, which is why human code review is needed as the last line of defense for code quality. At Google, code review is one of the most important phases of code consolidation, so the efficiency of the review has a direct impact on the overall development efficiency. Good single-test coverage can reduce the burden of reviewers, allowing them to focus their efforts on more important parts (e.g. code design).
+ **More frequent development releases:** Agile development's premise of continuous integration and continuous deployment is full-feast, quality-enhancing self-initiated testing. The efficiency gains of agile development for R&D have yet to unfold. But just being able to develop versions more quickly is already quite valuable.



### Negative Patterns and Common Misconceptions



Above, we mentioned a number of benefits of writing unit tests and related best practices. We've also listed down the common counter-patterns and misconceptions to help most families better avoid similar mistakes.



#### Antipatterns of testing (anti-patterns)



**Anti-pattern #1: Ice cream cone pattern**



End-to-end testing that focuses only on user-household visibility, and massive reliance on QA testing all produce the anti-pattern shown below. Unfortunately, this is also the most common pattern under the influence of past testing systems. Ice Cream Cone** Patterns Next, test suites typically run slowly, unreliably, and are difficult to use. **



! [](https://pic4.zhimg.com/80/v2-c99708f261a64544845fb749b43ba6d7_720w.webp)



Photo credit: Google Software Engineering



**The opposite mode two: Hourglass mode**



In Hourglass Mode, the project has tons of unit tests and telomere tests, but lacks integration tests. While it's not as bad as ice cream, it still leads to many telomere test failures that could have been captured faster and easier with a medium-sized set of tests for the same thing. The hourglass pattern occurs when modules are tightly connected to each other, making it difficult to instantiate dependencies individually.



! [](https://pic3.zhimg.com/80/v2-401c712cd94473865523a406e40395ba_720w.webp)



Photo credit: Google Software Engineering



#### Common Error Areas in Testing



**Common Misconception #1: User first, testing covers the user's needs sufficiently



The misconception is that the ultimate test is to test from the user's perspective, and that it is sufficient to cover all the functionality that the user wants. The result of this misconception is the ice cream cone reverse model. While the final functionality of software delivery is provided to the customer-user, the code that makes up the software itself is provided for the user (R&D) to read and needs to be maintained by the user. External users are users and internal users are users**. **



**Frequent error area two: full testing, saving 80% of the amount of test code, win hemp **



In the short term, not writing a single test can save 80% of the amount of test code and at least 50% of the development time. However, as soon as the project becomes complex, the time** doubles sooner or later. **If you wait until you really need to pay off the debt, it may be too late.



**Myth #3: People who write single tests are weak, I have never written a bug**!



This article may not be for you. However, software development is a team project, the code you write ends up in the hands of others to upgrade maintenance, no test coverage of the code is no one dare touch.

