---
title: Amazon AIGC 产品：构建生成式人工智能应用程序的最简单方式
description: 使用Amazon的新一代Trn1n实例，加速深度学习模型的训练，提高生成式人工智能应用程序的性能和效果。该实例具备高性能网络、可扩展性和成本效益，是开发人员和研究人员构建生成式人工智能应用程序的最简单方式。
tags:
  - Amazon AIGC
  - Trn1n实例
  - 生成式人工智能应用程序
  - 深度学习
  - 高性能网络
  - 可扩展性
  - 成本效益
  - 训练加速
  - 性能提升
date: 2023-07-20 19:50:26
---

# Amazon AIGC Products: The Easiest Way to Build Generative AI Applications

![Photo by [Colton Sturgeon](https://unsplash.com/@coltonsturgeon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/7744/0*Gwdgc_fcR7ZgGNnb)

## 介绍

亚马逊拥有数千名工程师致力于机器学习研究，因为这是他们未来成功的关键。通过使用人工智能和机器学习，亚马逊可以改进客户服务、提高运营效率并保持竞争优势。

亚马逊云技术发布了几款新的AIGC产品，包括Amazon Bedrock、Amazon EC2 Trn1n、Amazon EC2 Inf2、Titan AI和Amazon CodeWhisperer。这在市场上引起了很大的关注，许多技术巨头都试图进入这场游戏，成为人工智能领域的最新趋势。AIGC已成为最接近商业应用的技术出口，即将迎来爆发的蓝海市场。但问题是，谁将脱颖而出，突破AIGC的混乱局面呢？

## 亚马逊的家庭桶

Amazon Bedrock是一项新服务，可以通过API访问来自AI21 Labs、Anthropic、Stability AI和亚马逊本身的基本大型模型。Bedrock是用户构建和扩展基于AI的生成应用程序的基本框架，可以访问包括亚马逊的Titan FM在内的强大文本和图像大型模型功能。

亚马逊还正在测试新的Titan FM，并计划在未来几个月推出两个Titan模型。第一个是生成LLM，用于摘要、文本生成、分类、开放式问题回答和信息提取等任务。第二个是嵌入LLM，将文本输入转化为包含文本语义的数值表示。亚马逊还宣布推出由AWS Trainium提供动力的Amazon EC2 Trn1n实例和由AWS Inferentia2提供动力的Amazon EC2 Inf2实例。

Trn1实例可以节省50%以上的训练成本，比其他任何EC2实例都要多，使用Trn1实例可以帮助将训练最大的深度学习模型所需的时间从数月缩短到数周甚至数天。由Inferentia2支持的实例经过优化，适用于包含数千亿参数的大规模生成AI应用模型。亚马逊还宣布预览版的Amazon Code Whisperer是一款可以根据开发者的自然语言评论和集成开发环境（IDE）中的先前代码实时生成代码建议的AI编程伴侣。

## CodeWhisperer的好处

在试用阶段，AWS进行了一项生产力挑战，使用CodeWhisperer的参与者完成任务的速度平均提高了57%，成功率提高了27%，而不使用CodeWhisperer的参与者。这是生产力的巨大飞跃！

## 支持的语言和安全功能

CodeWhisperer可用于Python、Java、TypeScript、C＃等语言，还支持Go、Kotlin、Rust、PHP和SQL等十种新语言。它还具有内置的安全扫描（由自动推理提供动力），用于发现难以检测到的漏洞并建议修复。CodeWhisperer过滤掉有偏见或不公平的代码建议，并可以标记与开源代码类似的代码建议，客户可能希望参考或许可的代码。

## 如何入门

CodeWhisperer对个人用户免费。任何人只需使用电子邮件账户注册，即可在几分钟内开始使用，甚至不需要AWS账户。对于企业用户，AWS提供CodeWhisperer专业版，其中包括与AWS身份和访问管理（IAM）集成的单一登录（SSO）集成和更高的安全扫描限制。总之，Amazon CodeWhisperer是开发人员想要更高效地编写代码并节省时间的优秀工具。借助其基于AI的建议和安全功能，CodeWhisperer对于任何希望提高生产力的开发人员来说都是必备之选。

## Trn1n实例

由Trainium提供动力的Trn1n实例可节省高达50%的训练成本，优化了将训练分布在连接到每秒800Gbps的第二代Elastic Fabric Adapter（EFA）网络的多个服务器上的能力。客户可以在UltraClusters中部署Trn1n实例，在同一AWS可用区中扩展至多达30,000个Trainium芯片（计算超过6ExaFLOPS），具有PB级网络。包括Helixon、Money Forward和Amazon Search团队在内的许多AWS客户使用Trn1n实例，以帮助缩短将最大深度学习模型的训练时间从数月缩短到数周甚至数天，并降低成本。

## 新的网络优化的Trn1n实例

AWS宣布了新的网络优化的Trn1n实例的正式推出，其网络带宽达到1600Gbps，为大型网络密集型模型提供比Trn1n高20%的吞吐量。新的Trn1n实例采用第三代Elastic Fabric Adapter（EFA）网络，具有更高的带宽和更低的延迟，可实现更快的训练速度和更高的性能。

## Trn1n实例的优势

Trn1n实例提供了许多优势：

- 高性能网络：新的Trn1n实例通过第三代EFA网络实现了更高的网络吞吐量和更低的延迟，提供了出色的性能。

- 高度可扩展：使用Trn1n实例，客户可以在UltraClusters中扩展至多达30,000个Trainium芯片，以满足对大规模训练的需求。

- 成本效益：Trn1n实例通过优化训练成本，帮助客户节省高达50%的训练成本，同时提供卓越的性能。

- 加速深度学习：Trn1n实例的推出有助于缩短深度学习模型的训练时间，将数月的训练时间缩短到数周甚至数天。

## 如何使用Trn1n实例

使用Trn1n实例进行训练非常简单。您只需在AWS控制台或使用AWS命令行界面（CLI）选择Trn1n实例类型，并将其作为训练作业的目标实例即可。您可以根据实际需求配置实例数量和规模，并利用Trn1n实例的高性能网络和可扩展性来加速训练过程。

## Trn1n实例的应用

Trn1n实例广泛应用于各种深度学习场景，包括计算机视觉、自然语言处理、语音识别等。许多企业和研究机构使用Trn1n实例来加速模型的训练和推理，从而提高其人工智能应用的性能和效果。

总之，新的网络优化的Trn1n实例为深度学习模型的训练提供了更高的性能和更快的训练速度。借助其高性能网络、可扩展性和成本效益，Trn1n实例是开发人员和研究人员在人工智能领域中的强大工具。


