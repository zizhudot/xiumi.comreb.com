---
title: It's 2023, so why aren't SSRs as popular as expected?
description: It's 2023, so why aren't SSRs as popular as expected?
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

A study found that every one-second increase in website loading time will result in a 10% loss of users. In order to improve the page's second open rate, people from all walks of life continue to explore the optimization strategy, only in the browser field under the optimization has been unable to meet the requirements of the ultimate, we began to explore the direction of the server-side, and once let the [Server Side Rendering] this ancient concept of "red", and hype is hot.



Server-side rendering abbreviated SSR, full name Server Side Rendering, as the name implies is the work of rendering on the Server side. This approach is not only conducive to the first screen rendering, to improve the first screen response speed of SPA applications, but also to facilitate search engine crawling, which is conducive to SEO optimization. However, by 2023, SSR is not as popular as expected.



Some commented that most of the reasons for using SSR is to serve SEO, but now search engines have kept pace with the development of SPA written in a framework support is also good, so the need for SSR is not so great. There are also people who think that SSR is a pseudo-requirement, and that business logic and controllers load just as fast when they are separated.



But there are also comments that there are still a large number of users who can't have a good experience when accessing web pages because of the network environment or device conditions, so if we want to improve the experience of this part of the user, then SSR is an indispensable way to do so.



What is the real situation? What are the reasons that have prevented SSR from becoming the dominant development paradigm on the Web? Is this approach outdated in today's environment? What kind of business scenarios are more suitable for SSR? Open Source China invited two front-end leaders to listen to their views.



+ Liu Kui, community nickname kuitos. Liu Kui, community nickname kuitos, front-end engineer of Alipay Experience Technology Department, author of the open source micro-front-end program qiankun, is currently responsible for web infrastructure R&D related work in Ant.
    
+ Liu Yong, community nickname skypig, head of Node.js Infra in a big factory, core developer of EggJS / CNPM.
    



## I. SSR, not a pseudo-requirement



**Q1: In your experience, what types of projects and scenarios are more commonly used ** **SSR** **? Can you give some examples? **



**Liu Kui:** SSR is more commonly used for such websites that are very sensitive to first screen performance or have strong SEO requirements, such as:



+ E-commerce platforms: faster first screen rendering allows users to see product information faster, increasing the conversion rate of purchase.
    
+ Campaign pages: SSR can effectively improve the business results of marketing campaigns.
    
+ Portals: content-based sites usually have a stronger claim on SEO
    



**Q2: From your actual experience, what do you think are the advantages of ** **SSR** **compared to ** **CSR**** (****Client-side rendering****) mode? **



**Liu Kui:** From my personal experience, the biggest advantage is still in the first screen experience, SSR mode HTML loading process users can see the effective page content, this is basically CSR is difficult to do.



**Q3: Nowadays **** search engine **** already supports rendering, do you think there is still a need to use ** **SSR** ** because of ** **SEO** **? **



**Liu Kui:** Due to well-known reasons, domestic search engines do not support SPA type of application is not good, if you want your site can be better indexed by the crawler, basically still need to use SSR (or SSR variants) program.



**Q4: Some people believe that ** **SSR** ** is a pseudo-requirement to improve the first screen rendering performance, if the back-end service business logic and controller separation, the controller is divided into view controller and interface controller, call the same business logic. The first time you open the page, the front-end ** **JavaScript** ** load the data rendered on the page, and then request the interface to get the data when the user interacts. This solution is much better than SSR, which is in a performance hurry. How do you rate it? **



**Liu Kui:** This solution is still CSR in nature and cannot solve the problem native to CSR solutions: that is, the user must wait until the JS download is complete -> initiate an interface request -> JS gets the data and renders the page before they can see the valid content. In the more demanding network environment and user device conditions, this problem will be more obvious.



**Liu Yong:** according to the team's infrastructure maturity and business scenarios to do technical selection, these 2 programs are not absolutely superior or inferior, nor is it absolutely cut off, they can be combined into a program through the front-end engineering.



## Second, SSR, want to red a little difficult



**Q5: In the current situation, ****SSR** **and did not become the mainstream Web development model, you think the obstacles are? **Q5



**Liu Kui:** I think there are mainly these types of reasons:



+ **Technical complexity:** SSR requires server-side rendering and integration with front-end frameworks, which requires more technical knowledge for developers.
    
+ **SSR** **Bringing additional development and maintenance costs:** Relative to CSR, SSR solutions require the front-end to pay extra attention to server-side related development and operation and maintenance, such as how to write higher performance server-side rendering logic, how to deal with potential memory leaks, variable contamination, and other isolation issues, and how to do SSR disaster recovery (fallback to CSR in the event of a SSR failure) etc. All of these require extra resources and time investment from the team.
    
+ **Scenario Match:** A large number of services in China are distributed through small programs and APPs, and there are relatively few products with pure Web technology stacks, which is very different from the scenarios in foreign countries.
    



**Liu Yong:** First of all, SSR requires server resource costs, and in the context of cost reduction and efficiency, it will need to be combined with some infrastructure such as Serverless or edge computing to find a balance. At the same time, since it is the server side, there are certain requirements for operation and maintenance capabilities, and there are certain requirements for the technical accumulation of the front-end team.



Secondly, if the packaging and maintenance of the framework is not done well, it is very common for business students to write SSRs that are prone to memory leaks. Moreover, the current front-end framework has not been optimized for SSR scenarios, so if the first screen display is fast, but then you have to download the huge Bundle file, so the user interaction time is too slow, it is not worth it.



Finally, the evolution path problem, such as the ant side, they have been with the offline package of the upstream and downstream infrastructure are done very well, APP side, network side of the brother team to cooperate with the polishing. This model will have some defects, such as offline packets too much business competition, but on the first screen performance, SSR is not necessarily much better than it, then let them switch to SSR there will be no small resistance.



**Q6: There are comments that ** **SSR** ** is too expensive to develop and maintain, and they are turning to ** **CSR** ** Can CSR achieve the same effect as SSR? Are there any specific operational programs? **



**Liu Yong:** From the key point of the first screen performance, if CSR does not do some optimization, at least 3 serial HTTP requests, the first screen time is certainly not as good as SSR (interoperability time is not necessarily).



However, there are many corresponding solutions, such as ServiceWorker, offline packages and so on.



**Liu Kui:** From the point of view of first-screen rendering speed alone, CSR can be optimized in the following way if it wants to achieve the similar effect of SSR:



1. **First screen page static resource optimization:** through code cutting & lazy loading and other means, to ensure that the first screen needs JS/CSS is a minimized version, and through inlining and other ways to directly hit the HTML, to reduce the first screen rendering needs of network requests;
    
2. **Caching and **** preloading ****: **Use client-side caching and preloading and other mechanisms to improve the speed of the second visit;
    
3. **Use lighter weight frameworks:** Choose lighter weight front-end frameworks, so as to reduce the JS volume of the first screen and improve the loading speed;
    
4. **Optimize the response speed of key interfaces:** Optimize the response speed of interfaces for key content needed on the first screen to ensure that the front-end can render the page faster.
    



However, if there are additional SEO requirements, it may be difficult to achieve the same effect with simple CSR.



**Q7: How much would it cost to switch the original application directly to an ** **SSR** **integrated application? What would be the challenges for the development team? **



**Liu Kui:** The costs and challenges are as follows:



1. **Application transformation cost: **Most of the applications can not be directly run in the server-side environment, basically need to do a certain degree of transformation, such as eliminating the first screen rendering code in the dependence on the window, location and other browser-specific APIs, to build a JS for server-side runtime and so on.
    
2. **SSR** **Function R&D and O&M Challenges:** Teams with rich front-end and server-side development experience are rare in most companies. As mentioned earlier, SSR brings additional server-side development and O&M challenges, which also need to be considered by the front-end team.
    



## III. Maybe, SSR + CSR will be the new direction in the future?



**Q8: Now some sites use **** first screen server-side **** rendering, that is, for the user to start opening the page using the server-side rendering, which ensures that the rendering speed, and other pages using **** client-side rendering ****, so that the front and back end of the completion of the separation. Do you think this would be a more perfect solution that incorporates the advantages of both? **



**Liu Kui:** Yes, this is also the current best practice in the community, which can well retain the advantages of SSR and SPA applications.



**Liu Yong:** This is actually many years ago there are related practices, such as when Yunlong in the UC Scrat Pagelet is a similar practice, and even at that time to do the subsequent page is also through the server-side local rendering, on-demand update of the front-end page of the stage.



This approach in the industry has also seen some more recent practice: developers are very natural to write logic, do not care about what separation is not separation of things, in the front-end engineering layer of automatic splitting, SSG + SSR + CSR, some can be built statically directly in the construction stage of the processing, some can be rendered in the server-side service-side, the rest of the non-modest components directly rendered out of the front-end. All of these can be done, provided that the front-end engineering piece of the infrastructure is perfect enough, the R&D model is convergent enough.



As a final reminder, most SSR practices that I know of generally also block a short-lived CDN in the front, and then do thousands of modifications and subsequent business logic through CSR.



**Q9: How do you see the future development of ** **SSR** **? Will it be phased out with hardware upgrades, or will it become more and more popular with technology updates? **



**Yong Liu:** Optimization ideas are not obsolete, maybe one day we are familiar with the programming interface of SSR has changed, for example, when SSR was using nunjucks, ejs and other templates, and now it is react, vue. the future will also be a new technology, but it is likely to belong to the SSR of a practice model.



**Liu Kui:** In my experience, most of the time, new technology solutions will try to squeeze more from the hardware to get a better interactive experience, so there will be relatively "low-end" devices at any time, and this should not be solved (laughs).



In my opinion, the most important landing cost of SSR is still in the R&D and O&M of the server side, which is a big burden for the front-end team of most companies, and then the ROI is not high, leading to difficulties in landing SSR. However, with the development of Serverless, there are many almost "zero operation and maintenance" Serverless programs, which can greatly reduce the front-end team's operation and maintenance costs. At the same time, from the community trend, in recent years, the popular front-end frameworks are embracing Edge and SSR, such as Next.js, remix-run, Qwik, Astro, Fresh, etc. At the same time, React and other libraries are also embracing Edge and SSR. At the same time, libraries such as React have introduced streaming SSR capabilities for better performance performance. Through the integration and iteration of these framework technologies, not only can significantly reduce the R&D cost of front-end engineers developing SSR applications, but also further improve the performance effect of traditional SSR.



From the current trend, I think SSR will become more and more popular with the reduction of R&D and O&M costs.



**Q10: Combined with your project experience, how would you evaluate ** **SSR** ** this model? **



**Liu Yong:** Looking at the historical evolution of the front-end, it is SSR → CSR → SSR, which at a rough glance seems to be driving history backwards, but in reality it is not.



For example, when the front-end HTML + CSS + JS are all-in-one single file way, because the front-end did not have the ability to compile can only be written together; with the evolution of front-end engineering, the development of the development period into a multi-file way of organizing the construction of the automated processing of the mainstream; and then further appeared similar to the Vue SFC such a single-file way, this is to drive backward? Is this a step backward? No, it's not, but as the infrastructure improves, the user programming interface can be more intuitive, leaving things like performance and deployment to the tools.



So I think there are real scenarios for the SSR model, but at this stage, I think there are still a lot of practical performance and engineering issues that need to be resolved in order for it to land better.



**Liu Kui:** Although CSR can get a better first-screen experience, there is an obvious performance ceiling due to the functionality of the user's device. SSR, on the other hand, can be better utilized with edge computing.