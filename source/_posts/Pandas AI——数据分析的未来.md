---
title: Pandas AI——数据分析的未来
date: 2023-07-06 09:41:31
categories:
  - AI
tags:
  - ChatGPT
  - Pandas AI
  - 数据分析
description: 如果你热爱或者工作需要用到数据分析，那么Pandas AI将成为你最好的良师益友。
cover: https://s2.loli.net/2023/07/24/NhK5HPdoJY2Cc6Q.png
---

想象一下能够像你最好的朋友一样与你的数据交谈。这就是 Pandas AI 所做的！这个 Python 库具有生成人工智能功能，可以将您的数据框变成对话者。不再需要无休止地盯着行和列。

借助 Pandas AI，您可以将数据分析和操作提升到一个新的水平。把它想象成一个超级英雄的助手——它可以帮助你拯救世界，让你的生活更轻松。

**Pandas AI有着无限的可能性**。想象一下，拥有一个可以编写自己的报告的数据框架，或者可以分析复杂数据并为您提供易于理解的摘要的数据框架。

无论您是经验丰富的数据分析师还是初学者，本篇文章都将为您提供深入Pandas AI世界所需的所有工具。所以坐下来，让我们探索Pandas AI提供的令人兴奋的可能性！

- 官方GitHub — [https://github.com/gventuri/pandas-ai](https://github.com/gventuri/pandas-ai)
- 代码 — [https://colab.research.google.com/drive/1rKz7TudOeCeKGHekw7JFNL4sagN9hon-?usp=sharing](https://colab.research.google.com/drive/1rKz7TudOeCeKGHekw7JFNL4sagN9hon-?usp=sharing)

## 使用 pip 安装 Pandas AI

> pip install pandasai

我们的DataFrame包含有关各个国家/地区的信息，包括其GDP（以百万美元为单位）和幸福指数得分。它由10行和3列组成：

![](https://s2.loli.net/2023/07/24/BgPVYX4dk7LMOFm.png)

## 使用OpenAI导入Pandas AI

在该步中，我们将导入之前安装的pandasai库，然后导入LLM（大型语言模型）功能。截至2023年5月，pandasai仅支持OpenAI模型，我们将利用该模型来理解数据。

![](https://s2.loli.net/2023/07/24/V8a4EthMPYKeUzq.png)

要使用OpenAI API，您必须生成自己的唯一API密钥。如果您还没有这样做，您可以在平台的官方网站上轻松创建一个帐户。创建帐户后，您将立即收到 5 美元的积分，可用于探索和试验 API。

## 初始化Pandas AI并提出问题

之后，我们将向Pandas AI提供OpenAI模型并提出各种问题。

![](https://s2.loli.net/2023/07/24/gjk2FbHh1Tr3EaM.png)

使用 pandas_ai.run 时，需要两个参数：您正在使用的数据框和您正在寻求答案的问题，它会根据提供的数据框返回前 5 个最幸福的国家/地区。

## 提出复杂的问题

神奇的是，它确实根据我提出的问题绘制了图表。

![](https://s2.loli.net/2023/07/24/IGSTKrFcXlDgLh4.png)

让我们执行一项复杂的任务，从以下数据集中删除 NAN 值：

![](https://s2.loli.net/2023/07/24/PBCs5TpSryQ92dt.png)

这是我们得到的输出结果：

![](https://s2.loli.net/2023/07/24/8suCtHS36DIgGQx.png)

但是当我再次输入 df 变量时，它确实从数据集中删除了那些 NAN 值，从而完全删除了该行。

![](https://s2.loli.net/2023/07/24/BwbaS16vYKHWXN7.png)

pandasai 库提供了广泛的可能性，您可以通过访问我之前分享的官方存储库页面来探索所有这些可能性。

> 需要注意的是，与 pandasai 合作涉及 OpenAI 定价，您可以在他们的网站上找到最新的定价信息。截至 2023 年 5 月，定价约为每 1000 个代币 0.0200 美元（对于 GPT-3.5-Turbo 模型）。提出问题时，重要的是要记住整个数据帧每次都会与问题一起传递，因此它可能不是处理大型数据集的理想解决方案。
