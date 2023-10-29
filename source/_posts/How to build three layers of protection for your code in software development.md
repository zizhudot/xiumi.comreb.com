---
title: How to build three layers of protection for your code in software development
description: How to build three layers of protection for your code in software development
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---


In the application process of DevSecOps, static analysis tools undertake a very important task of looking after code quality and security in the development phase. In this paper, based on the differences in the development environment, code characteristics, and inspection tool capabilities in different locations of the development process, we propose the need to deploy inspection tools according to local conditions to form a progressive three-layer code security defense system, so as to improve the overall security of the application software, and at the same time, effectively implement the strategy of security left shift to reduce the cost of maintenance of security issues.

## 1\. DevSecOps

In recent years, the proportion of DevSecOps introduction in large enterprises has been increasing year by year, from 41.3% in 2020 to over 63.5% in 2022, with a compound growth rate of over 20%.

The term DevSecOps was first coined by Gartner in 2012 and has gradually become a hot topic in software development over the last few years.DevSecOps bridges the gap between developers, testers, security teams, and Ops teams; it improves communication and collaboration between teams, with the goal of delivering faster and more efficiently.DevSecOps, which adds to DevOps, is a new approach to software development. DevOps adds the activity of security to the foundation of DevOps, which improves security while guaranteeing rapid development and rapid deployment, embedding security into the application to be able to respond to security threats more quickly.

The following diagram shows the nine phases of the DevSecOps software lifecycle as defined by the U.S. Department of Defense (DOD): Plan, Develop, Build, Test, Release, Deliver, Deploy, Operate, and Monitor. Security is embedded in each of these phases.The DevSecOps lifecycle is highly adaptable and has many feedback loops to drive continuous improvement.DevSecOps poses new challenges to traditional software development in terms of management philosophies, management methodologies, organizational structures, development processes, software development, development platforms, tool integrations, and corporate culture.


In the application process of DevSecOps, static analysis tools assume a very important role of code quality and security care in the development phase. This paper focuses on the important role of software static analysis tools in the development domain of DevSecOps.

## 2\. DOD DevSecOps

The U.S. Department of Defense (DoD) has been publishing a series of documents related to DevSecOps since 2021: the DoD Enterprise DevSecOps Strategy Guide, DoD Enterprise DevSecOps Fundamentals, DevSecOps Reference Designs, DevSecOps Operations Manual, and other related supporting documents.

This May was supplemented with the DevSecOps Foundation Guide: activities and tools. In this document the security activities and corresponding tools that need to be accomplished during the DevSecOps lifecycle are more clearly defined. The security activities involved are listed in the following table:

| Security Activities | Phases | Dependent Tools |
| --- | --- | --- | --- |
| Task-Based Cyber Risk Assessment | All | Task-Based Cyber Risk Assessment Tools |
| Threat Modeling | Programs | Threat Modeling Tools |
| **Code Commit Scanning** | **Development** | **Code Warehouse Security Plugin** |
| Secure Code Development | Development | IDE |
| **Static Code Scanning Before Commit** | **Development** | **IDE Security Plugin** |
| Dependency Component Vulnerability Checking | Build | Dependency Checking / Bill of Materials Checking Tools |
| **Static Application Security Testing and Scanning (SAST)** | **Build** | **Static Application Security Testing and Scanning Tool (SAST)** |
| Database Security Testing | Testing | Security Compliance Tools |
| Dynamic Application Security Testing and Scanning (DAST) | Testing | Dynamic Application Security Testing and Scanning Tool (DAST) or Interactive Application Security Testing Tool (IAST) |
| Interactive Application Security Testing (IAST) | Testing | Dynamic Application Security Testing and Scanning Tool (DAST) or Interactive Application Security Testing Tool (IAST) | Manual security testing (e.g., penetration)
| Manual Security Testing (e.g. penetration testing) | Testing | Various tools and scripts (may include network security testing tools) |
| Service Security Testing | Testing | Security Compliance Tools | Post-Deployment Security Scanning | Post-Deployment Security Scanning | Post-Deployment Security Scanning
| Post-Deployment Security Scanning | Deployment | Security Compliance Tools |
| Compliance Monitoring (Resources and Services) | Monitoring | Compliance Tools, Operations Kanban |
| Compliance Monitoring | Monitoring | Compliance Tools, Operation Kanban |
| Database Monitoring and Security Auditing | Monitoring | Compliance Tools, Operations Kanban | | Compliance Monitoring
| Runtime Application Security Protection (RASP) | Monitor | Security Compliance Tools |
| System Security Monitoring | Monitoring | Information Security Continuous Monitoring (ISCM) | SBOM Software Composition Analysis
| SBOM Software Composition Analysis | Late Build | SBOM and Software Factory Risk Continuous Monitoring Tools |
| SBOM and Software Factory Risk Continuous Monitoring Tool | SBOM and Software Factory Risk Continuous Monitoring Tool | SBOM and Software Factory Risk Continuous Monitoring Tool | SBOM and Software Factory Risk Continuous Monitoring Tool
| Interface Security Testing | Build | API Security Testing Tools |
| Cooperative and Adversarial Testing | Operations | Cooperative and Adversarial Testing Tools | Continuous Network Operations Testing | Continuous Network Operations Testing | Continuous Network Operations Testing
| Continuous Network Operations Testing | O&M | Continuous Network Operations Testing Tools |
| Engineering Obfuscation | O&M | Engineering Obfuscation Tools |

From this activity table we can see that there are three key security checkpoints during the development process, which we have highlighted in red in the table), and the information about these three checkpoints from the documentation has been combined into the following table:

| Activity | Baseline | Description | Inputs | Outputs | Dependencies | Tools
| --- | --- | --- | --- | --- | --- | --- | --- | ---
| Static Analysis of Code Before Commit | Requirements | Scans and analyzes code as developers write it. Notify developers of potential code weaknesses and make recommendations for remediation. | Source Code, Known Weaknesses | Discover weaknesses in code | IDE Plugin |
| Code commit checking | Requirements | Check for sensitive information in changes before pushing them to the code silo. If suspicious content is found, it notifies the developer and blocks the commit. | Local Commits | Security Issues and Warnings Detected | Code Warehouse Security Plugin | Static Application Security Testing | Static Application Security Testing | Static Application Security Testing | Static Application Security Testing
| Static Application Security Testing | Requirements | Perform Static Analysis Checks on Software Systems | Source Code, Known Security Issues and Vulnerabilities | Static Inspection Reports and Recommendations for Fixes | Static Analysis Tools | Static Analysis Tools

From this table we can see that the appropriate static analysis tests need to be completed at the following test points during the development phase of the code:

+ In the IDE, through the IDE security plug-in, security inspections need to be completed for code that needs to be submitted;
+ In the code warehouse, through the security plug-in, the code submission scanning, which is often referred to as "access control";
+ In the build, through the static analysis tool, to complete the static application security testing and scanning (SAST).

The DevSecOps Essentials Guide: Activities and Tools only gives a rough idea of the activity requirements and tools needed for the process testing points, and does not give a specific process fusion and how to select tools for the different testing points.

## 3\. OWASP DevSecOps

The Open Worldwide Application Security Project (OWASP) is a non-profit foundation dedicated to improving software security. The Foundation is committed to improving software security through its community-led open source software projects, hundreds of chapters worldwide, tens of thousands of members, and local and global conferences.

The OWASP DevSecOps Guideline (OWASP DevSecOps Guideline) guides us on how to implement a security pipeline and use best practices, and describes the tools that can be used in this matter.

The guide also dedicates a special section to the pre-commit process in DevSecOps practices. This is shown in the figure below:


This diagram clearly gives the checking process for pre-commit and gives two types of checks that need to be done in pre-commit:

+ Ensure that there are no password or key issues in the code;
+ The code follows the Linter rules.

I can't find a suitable Chinese translation for Linter, which is a small ball of lint or fibers formed on clothes after they are washed in a washing machine due to the friction of rolling the fibers together. In the past, people wanted to get rid of these extra "balls", but later they invented a magic tool called Linter, which could remove these "balls" with a single roll.

In 1978, Stephen C. Johnson, working at Bell Labs, was debugging his C project when he thought why not make a tool that could tell him what was wrong with the code he was writing. This tool is also known as Linter. Linter is a static analysis tool, mainly used to find syntax errors, potential bugs, code style, etc. in the code. The various tools we commonly see named linter are this type of static checking tool. Almost every language has a Linter tool for its own language, such as the familiar:

+ The role of Linter is given in the OWASP DevSecOps Guide:
    
    + Detect errors in code and errors that could lead to security vulnerabilities;
    + Detect formatting or styling issues and make code more readable, resulting in more secure code;
    + detects suggested best practices;
    + can improve the overall quality of the code;
    + Maintaining code is easier because everyone follows the same linting rules.
+ The OWASP DevSecOps Guide defines static inspection tools as Linter and Advanced Static Inspection Tools, depending on the problem being inspected:
    
    + Linter tools are the most basic form of static analysis. Using the linter tool helps to identify common errors such as:
        
        + Array index out of bounds;
        + Null pointer dereferences;
        + (potentially) dangerous data type combinations;
        + Inaccessible code (dead code);
        + Non-portable structures.
    + Advanced static analysis tools. Advanced static analysis tools typically provide:
        
        + Pattern-based inspections.
        + Quality and complexity metrics;
        + Developer-oriented best practice recommendations;
        + Support for a wide range of safety and security-focused coding standards;
        + Support for multiple safety- and security-focused coding standards; + Used to develop safety-critical applications, e.g., out-of-the-box authentication.

The OWASP DevSecOps Guide gives differences in the types of defects checked by different tools, mainly to illustrate that different types of inspection tools need to be configured at different points of detection.

## 4\. Security Left Shift

Capers Jones in "Application Software Measurement: A Comprehensive Analysis of Productivity and Quality" explains that from a software engineering practice perspective, it shows that most problems are introduced during the coding phase, and also that as defects are discovered later in the development process, the more expensive they are to fix.

So we hope that by integrating testing after development into the development process, we can effectively reduce the cost of fixing defects introduced during development.

+ Testing shifts left to the development phase  
    
+ Testing shifts left, defects are reduced in the development phase  
    

After the concept of DevSecOps was introduced, it was natural to come up with the concept of **"secure left shift "**. "Security Left Shift" (as defined in the OWASP DevSecOps Guide) is an approach or solution that embeds security into the development process and considers security from the initial steps of application or system design. In other words, security is accountable to everyone engaged in the software development and operations process. Of course, security is a profession and we need highly skilled people to play security-related roles; but in this approach, any designer, software architect, developer, DevOps engineer, and ... are responsible for security along with security personnel.

From this description we can see that the security left shift includes several specific activities:

+ Security starts with design and continues throughout the process;
+ All personnel are involved in safety activities;

Based on our previous understanding of the U.S. Department of Defense's DevSecOps Foundation Guide: Activities and Tools, and OWADSP's OWASP DevSecOps Guidance, there are three checkpoints during the coding phase of the development process: the IDE, the gatekeeper, and continuous build (CI). According to the concept of "safe left", can we also **"go all the way to the left "**, put the static checking tool into IDE or gated, realize the left shift of the static checking tool, so as to remove the checking part of the continuous build (CI)? Is it possible to go all the way to the left?


## 4.1. the idiomatic story "Adapting to the local context"

It's been a while since I've told a story in a blog. China has a long history of sages and sages who have blended all sorts of truths and philosophies about people and their behavior into stories that are easy to understand and refined into enjoyable idiomatic stories that have a long history, so that ordinary people can remember the essence of these philosophies and pass them down from one generation to the next. In addition to the songs themselves, what people prefer to dig into are the various poignant stories contained in the songs.

At the end of the Spring and Autumn Period, the king of Chu listened to slander and killed Wu Zixu's family, and Wu Zixu fled to the state of Wu. In order to avenge himself with the help of the state of Wu, Wu Zixu gave the king of Wu the following advice: in order to make the country rich and strong, and the people stable, first of all, we have to build a high wall, so that we can strengthen the defense force, so that other countries dare not invade the country. The king of Wu told him that in order to make the country rich and strong and the people stable, the first thing to do was to build high walls, so as to strengthen the defense force, so that other countries would not dare to invade the country. At the same time, we also need to develop agriculture, only with the development of agriculture, the country can be rich and strong, the people can live and work in peace and contentment, and the generals can have a full of