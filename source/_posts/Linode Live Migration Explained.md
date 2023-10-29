---
title: Linode Live Migration Explained
description: Linode Live Migration Explained
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

When developers deploy workloads to cloud computing platforms, they often don't need to think about the underlying hardware that runs those services. Hardware maintenance and physical constraints are often invisible in the idealized image of the cloud, yet hardware inevitably requires maintenance from time to time, which can lead to downtime. To avoid such downtime being passed on to our customers and to truly realize the promise of the cloud, Linode offers a tool called Live Migration.

With Live Migration, Linode instances can be moved between physical servers without service interruption. When moving Linode instances through the Live Migration tool, the migration process is completely invisible to the processes running in the Linode instance. If the hardware of one host requires maintenance, all Linode instances on that host can be seamlessly transferred to another host through live migration. Once the migration is complete, the physical hardware can begin to be repaired, with no customer-impacting downtime.

This has become an almost defining technology and a turning point between cloud and non-cloud technologies. In this article, we will delve into the details behind this technology.

* * *

* * * To celebrate Linode joining the Akamai Solutions family, sign up for Linode now and get $100 worth of free access to as many services as you want from the Linode Cloud Platform. Click here to learn more and sign up today ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ **


* * * *

## Live Migration Works

Similar to most new projects, Linode's live migration started like this: a lot of research, a series of prototypes, and a lot of help from colleagues and management. Our first step was to investigate how live migration would be handled by QEMU, a virtualization technology used by Linode and a feature of QEUM. So our team's focus was to bring this technology to Linode, rather than reinventing a similar technology.

So how exactly is live migration technology implemented in a QEMU way? The whole process is divided into the following four steps:

1. start the target QEUM instance, which has exactly the same parameters as the source QEUM instance to be migrated.
2. Perform a live migration of the disks. Any changes made to the disk contents during the data transfer are also committed to the target disk.
3. live migration of memory data. Any changes made to the contents of memory during the migration process are also committed to the target memory. If disk contents are changed during this process, the changes are also committed to the target QEUM instance's disks.
4. Execute the cut point. The source and target QEMU instances will pause when QEMU confirms that there are enough memory pages to safely perform the cutover.QEMU copies the last few pages of memory data and machine state, which consists of the CPU cache and the next CPU instruction. QEMU will then get the target up and running so that the target instance can resume running from the state it was in when the source instance stopped.

These steps summarize the execution of a QEMU live migration. However there is still a need to specify exactly how the target QEMU instance should be started by including many manual operations. In addition, each operation in the above process must be executed at the correct time.


## Linode's approach to real-time migration

After analyzing the techniques already implemented by QEMU developers, it is time to think about the way we will actually give the live migration to Linode, and this answer is the main focus of our work.

In step 1 of the live migration workflow, the target QEMU instance needs to be started in order to accept the incoming migration connection. In implementing this step, our initial idea was to get the [configuration file]() of the current Linode instance and subsequently apply it to the target machine. In theory this should be simple, but further reflection reveals that in practice it is much more complex. In particular, while a configuration file can tell us how a Linode instance was started, it does not necessarily provide a complete description of the complete state of the started Linode instance. For example, a user can connect a [block storage]() device by hot-plugging it after the Linode instance has finished booting, but this is not recorded in the configuration file.

In order to create a QEMU instance on the target host, the currently running QEMU instance must be profiled. We profile a running QEMU instance by examining the [QMP]() interface, which provides us with a wealth of information related to the layout situation of the QEMU instance, but it does not help us understand what is happening inside the instance from the perspective of the guest system. For example, for local SSDs and block storage, it can only tell us where the disks are linked to and which virtualized PCI slot the virtual disk is attached to. After querying QMP and examining and analyzing the QEMU interface, a Profile can be constructed to describe how to create an identical instance at the target location.

On the target computer, we will receive a complete description of what the source instance actually looks like, and can then faithfully rebuild the instance at the target location, but there is one difference. This difference lies mainly in the fact that the target QEMU instance uses an option at startup that allows QEMU to accept incoming migrations.

At this point, the process of documenting the live migration is essentially complete, and it's time to look at how QEMU implements these operations. the QEMU process tree consists of a control process and multiple worker processes, one of which is responsible for tasks such as returning QMP calls or handling the live migration, while the others need to be mapped one-to-one to a guest CPU. the guest environment is isolated from the QEMU side of the functionality and behaves similarly to a separate system. The specific behavior is similar to that of a standalone system.

In this sense, we need to deal with three layers:

+ Layer 1 is the management layer;
+ Layer 2 is part of the QEMU process that handles all operations;
+ Layer 3 is the actual guest layer, responsible for interacting with Linode users.


Once the target instance is up and ready to receive incoming migrations, the target hardware will tell the source hardware to start sending data. The source will start processing upon receiving this signal and will tell QEMU in software to start transferring disk contents. The software autonomously monitors the progress of the disk transfer to check if the transfer operation is complete and automatically starts to migrate the memory contents after the disk transfer is complete. At this point, the software still monitors the progress of the memory migration and automatically switches to the cutover mode when the memory migration is complete. The whole process is done through Linode's [40Gbps network](), so network operations can be done quickly.

## Cutover: The Critical Link

The cutover operation is the most important part of the real-time migration process, and only when it is understood can the real-time migration operation be fully understood.

In the cutover point state, QEMU has confirmed that it is ready for all preparations and can be cutover and run on the target computer. The source QEMU instance puts both ends on hold, which means:

1. the guest system is "time-stopped". If the guest system is running a time synchronization service (such as NTP), NTP will automatically resynchronize the time after the migration is complete. This is because the system clock will fall behind by a few seconds.
2. Network requests stop. If the network request is a TCP request (e.g., SSH or HTTP), there is essentially no perceptible connection interruption; if the network request is a UDP request (e.g., streaming video), a small number of dropped frames may result.


Since both time and network requests are stopped, we would like the cutover to be completed as quickly as possible. However there are a few checks that need to be made to ensure a successful cutover:

+ Ensure that the live migration completes smoothly and without errors. In case of an error, a rollback is performed to unsuspend the source Linode instance from further operations. We experimented a lot with this and resolved many errors during development, and while this caused a lot of headaches for us, they were eventually resolved successfully.
+ Ensuring that the network on the source instance was shut down and properly connected on the target instance.
+ Make it clear to the rest of our infrastructure which physical computer the migrated Linode instance is running through.

Due to the limited time available for the cutover process, we would like to do the above as quickly as possible. Once these issues have been resolved, you can proceed with the cutover. The source Linode instance will automatically receive the "cutover complete" signal and get the target instance up and running. The target Linode instance resumes running from the state in which the source instance was suspended. The rest of the contents of the source and target instances are cleaned up. If the target Linode instance needs to be live migrated again at some point in the future, the above steps are repeated.

## Edge case overview

Most of the process of live migration was straightforward to implement, but the development of the feature itself was extended considerably after taking the edge case into account. The successful completion of this project was due in large part to the management team, who believed in the great vision of the tool and provided all the resources needed to accomplish the task, and of course the large number of employees who believed in the successful completion of the project.

We have encountered many edge cases in these areas:

+ Coordination efforts related to live migration by developing in-house tools for Linode customer support staff and hardware operations and maintenance teams. These tools were more similar to other tools of the same type that we were using at the time, but there were slight differences and we put a lot of development work into them:
    + The tool had to be able to automatically examine all the hardware facilities inside the data center and thus determine which hosts could be the best targets for each Linode instance that needed to be migrated. Relevant specifications to consider when making this selection and decision include available SSD storage space and memory allocation.
    + The physical processor of the target computer must be compatible with the incoming Linode instance. In particular, the CPU must have certain features (which we will refer to as CPU tags) that are essential for the software that the user is running. One such feature is AES, for example, which provides hardware-accelerated encryption-based capabilities. The target computer CPU for the live migration must support the same CPU tag as the source computer. We found this to be an extremely complex edge use case, and the approach we took is described below.
+ Gracefully handle failures including end-user intervention or loss of network connectivity during the live migration process. These are also described in more detail below.
+ Staying abreast of changes to the Linode Platform itself, which is an ongoing, long-term process. For each current and future feature supported by the Linode platform, we need to ensure that these features are compatible with live migration. For more information, please continue reading below.


## Failure Handling

There is a topic in software that is rarely discussed: handling failure gracefully. Software should at least be able to "run". In order to achieve this, a lot of development work is often required, and the same is true for the development of live migration functionality. We spent a lot of time thinking about what to do if the tool doesn't work, and how to gracefully handle the situation. We considered a number of scenarios and identified specific ways to respond:

+ What if a customer wants to access a feature of Linode from [Cloud Manager]()? For example, the user might restart Linode or connect Block Storage Volume for the instance.
    + Solution: The customer is perfectly capable of doing this. The live migration would be interrupted and the processing could not continue. This handling is appropriate because live migration can be retried later.
+ What if the target Linode fails to start?
    + Solution: The source hardware will be notified and another hardware will be automatically selected within the data center via a specially designed internal tool. The Ops team will also be notified so that the failed target hardware can be investigated. This has happened before in production environments and our live migration can handle it without any problems.
+ What if network connectivity is lost during migration?
    + Solution: Autonomously monitor the progress of the live migration and if no progress has been generated in the past minute, the live migration will be canceled and the Ops team will be notified. This has never happened outside of a test environment, but we are well prepared for this scenario.
+ What happens if the rest of the Internet is disconnected, but the source and target hardware are still running and communicating, and both source and target Linode instances are running normally?
    + Solution: If the live migration has not proceeded to the critical section, the live migration will be stopped and retried at a later time.
    + If it has progressed to the critical section, the migration will continue. This is important because the source Linode has been suspended and the target Linode needs to be in a started state to continue the resume operation.

These scenarios have been simulated in a test environment and we believe that the above behaviors are also the best responses for different situations.

## Keeping pace with technology changes

After hundreds of thousands of successful live migrations, we can't help but wonder, "When will the development of live migration end?" Over time, the technology of live migration will become more widely used and will continue to be refined, so it seems like the project will go on forever. One way to answer this question is to consider when most of the work on the project will end. The answer is also simple: our work will continue for a long time to come in order to get reliable, trustworthy software.

Over time, new features will be added to Linode, and we may have to continue working to ensure that live migrations are compatible with those features. Introducing some new features may not require new development work around live migration, but we may still need to test that the feature works as expected. For some features, the necessary compatibility testing and work around live migration may need to be done in the early stages of development.

Similar to almost all other software, there is always a better way to implement the same thing through continuous research. For example, in the long run, developing a more modular integration approach for the live migration functionality would certainly reduce the maintenance burden. Or we might even be able to incorporate live migration related functionality into the underlying code, making it an out-of-the-box Linode feature.

Our team has considered all of these options and is confident that the tools that drive the Linode platform are alive and well, and will continue to work to evolve and develop them.

* * * *


Translated with www.DeepL.com/Translator (free version)