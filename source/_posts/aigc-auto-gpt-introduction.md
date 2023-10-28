---
title: AIGC — Auto-GPT 介绍
description: Auto-GPT 的安装过程包括克隆仓库、安装依赖和更新环境变量文件等步骤。本指南将指导您完成安装 Auto-GPT 所需的所有步骤。
tags:
  - Auto-GPT
  - 安装
  - 依赖
  - Python
  - GitHub
  - OpenAI
  - PINECONE
  - ElevenLabs
  - AI
  - 语音模式
  - 环境变量
  - GPT-4
  - GPT-3.5
  - DALL-e
  - Stable Diffusion
date: 2023-07-17 19:50:26
---

## AIGC — Auto-GPT 介绍

### 展示**自主 AI**

![](https://cdn-images-1.medium.com/max/2000/1*NKkmD0SKFoV9QGQGnBZdTg.png)

最近推出的基于 GPT 的应用程序名为 Auto-GPT，现已在 GitHub 上可用（[https://github.com/Torantulino/Auto-GPT](https://github.com/Torantulino/Auto-GPT)），其创新特性是自主生成“自我提示”。

Auto-GPT 的自我提示功能引起了许多人的好奇，促使他们反思在相对短的时间内人工智能技术的快速发展。

以下是 Auto-GPT 创建和部署使用 React 和 Tailwind CSS 的网站的示例：

![](https://cdn-images-1.medium.com/max/2000/0*Xpcz2LoE6RYNwsD4)

## 什么是 Auto-GPT

Auto-GPT 是一个开源的 Python 应用程序，由名为“Significant Gravitas”的开发者在 GitHub 上发布。利用 GPT-4 的功能，这个开源工具使得人工智能能够独立运行，消除了持续用户输入的需求。

Auto-GPT 是一个尖端的开源应用程序，展示了 GPT-4 语言模型的潜力。该程序由 GPT-4 提供支持，连接 LLM 的“思维”以自主完成任何给定的目标。作为完全自主的 GPT-4 操作的开创性实例，Auto-GPT 扩展了人工智能的能力边界。

在为 Auto-GPT 设定一个总体目标后，它会继续执行一步一步的操作以实现该目标。这种方法基于“人工智能代理”的概念，可以独立浏览互联网并在计算机上执行任务，消除了持续监督的需求。

## Auto-GPT 的特点

它具有以下特点：

- 访问互联网进行搜索和收集信息

- 管理长期和短期记忆

- 利用 GPT-4 实例生成文本

- 连接到广泛使用的网站和平台

- 使用 GPT-3.5 进行文件存储和摘要

## Auto-GPT 的要求

- Python 3.8+

- OpenAI API 密钥

- PINECONE API 密钥

可选项：

- ElevenLabs 密钥（如果你想让人工智能说话）

Pinecone 是人工智能的长期记忆数据库（[https://www.pinecone.io/](https://www.pinecone.io/)）。

## Auto-GPT 的安装

假设满足所有要求，执行以下步骤：

### 克隆 Auto-GPT 仓库

```shell
$ git clone https://github.com/Torantulino/Auto-GPT.git
```

### 安装所需的依赖

```shell
    $ pip install -r requirements.txt
```
### 更新环境变量文件

将 .env.template 重命名为 .env，并填写你的 OPENAI_API_KEY。如果你计划使用语音模式，请同时填写你的 ELEVEN_LABS_API_KEY。

* 你可以从以下位置获取 OpenAI API 密钥: [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys).

* 你可以从以下位置获取 ElevenLabs API 密钥: [https://elevenlabs.io](https://elevenlabs.io/)。 你可以在网站的“Profile”选项卡中查看你的 xi-api-key。

### 运行 main.py 文件

在终端中运行 main.py Python 脚本：

```shell
    $ python scripts/main.py
```

在 AUTO-GPT 的每个操作之后，输入“NEXT COMMAND”以授权它们继续执行。要退出程序，请输入“exit”并按 Enter 键。

### 语音模式

要使用语音模式，运行以下命令：

```shell
    $ python scripts/main.py --speak
```

### Logs collection

你将在 ./logs 文件夹中找到活动和错误日志，要输出调试日志：

```shell
    $ python scripts/main.py --debug
```

## Caution! Continuous Mode ⚠️

此模式允许 Auto-GPT 在没有用户授权的情况下运行人工智能，100% 自动化。不推荐使用连续模式。它有潜在的危险，可能导致你的人工智能永远运行或执行你通常不会授权的操作。请自行承担风险。

在终端中运行 main.py Python 脚本：

```shell
    python scripts/main.py --continuous
```

要退出程序，请按 Ctrl + C。

## 图像生成

默认情况下，Auto-GPT 使用 DALL-e 进行图像生成。要使用 Stable Diffusion，需要一个 [HuggingFace API Token](https://huggingface.co/settings/tokens) is required.

获得令牌后，在你的 .env 文件中设置以下变量：:

```shell
    IMAGE_PROVIDER=sd
    HUGGINGFACE_API_TOKEN="YOUR_HUGGINGFACE_API_TOKEN"
```

## 限制

Auto-GPT 目前处于实验阶段，旨在展示 GPT-4 的能力，尽管存在一些限制：:

* 它不是一个完善的应用程序或产品，而是一个实验性项目。

* 在复杂的现实商业环境中的性能可能不够理想。如果你在这种情况下取得成功，请随时分享你的发现！

* 运营成本可能很高，因此建议与 OpenAI 确定并监控你的 API 密钥限制。

## 结论

* Auto-GPT 的架构基于 GPT-4 和 GPT-3.5，通过 API 进行连接；

* Auto-GPT 可以自主迭代，通过自我批判的审查、构建在先前工作基础上以及整合提示历史记录来提高输出的准确性；

* Auto-GPT 具有内存管理功能，包括使用 Pinecone 数据库进行长期存储、上下文保留和基于此进行决策的改进。
