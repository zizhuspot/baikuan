---
title: 分步指南：在 CPU 上运行免费的强大人工智能编码 LLM
date: 2023-09-19 09:11:24
categories:
  - AI
tags:
  - 分步指南
  - LLM
  - Replit Code Instruct
description: 在这篇博文中，我们将探讨如何使用 Replit Code Instruct 和 ctransformers 库设置和运行基于 CPU 的计算。从设置必要的环境到运行实际计算，我们将一步步指导您完成整个过程。
cover: https://s2.loli.net/2023/09/20/RNeZJwhd63DLlWQ.png
---

Replit Code Instruct 是一款功能强大的工具，可根据自然语言提示生成代码片段、解释和教程。它以前依赖 GPU 进行复杂的计算，**而现在 ctransformers 库允许仅使用 CPU 进行类似的计算**。这一进步为使用不同硬件设置的开发人员提供了更多可能性，彻底改变了编程和人工智能领域。

在这篇博文中，我们将探讨如何使用 Replit Code Instruct 和 ctransformers 库设置和运行基于 CPU 的计算。从设置必要的环境到运行实际计算，我们将一步步指导您完成整个过程。因此，如果你有一台 CPU 驱动的机器，就准备好进入 Replit Code Instruct 的推理世界，发掘它的潜力吧！

官方 GitHub 代码库 - [链接](https://github.com/abacaj/replit-3B-inference)

## 教程开始！💨
首先，我们需要按照以下步骤设置环境：
 
###  — 步骤 1 —

打开终端并执行以下命令，将 GitHub 中的“replit-3B 推理”存储库克隆到本地计算机上

```
git clone https://github.com/abacaj/replit-3B-inference
```

###  — 步骤 2 —
导航到克隆目录
```
cd replit-3B-inference
```

###  — 步骤 3 —
创建一个名为“env”的虚拟环境，并使用以下命令将其激活：
```
python -m venv env && source env/bin/activate
```

###  — 步骤 4 —
通过运行以下命令安装所需的依赖项：
```
pip install -r requirements.txt
```

###  — 步骤 5 —
执行提供的脚本 `download_model.py` ，以下载量化的模型权重。
```
python download_model.py
```
或者，您可以使用此链接直接从拥抱脸网站下载模型砝码。下载该文件后，创建一个名为“models”的新文件夹，并将下载的权重文件保存在其中。完成后，复制整个“模型”文件夹并将其粘贴到克隆的存储库中。这可确保模型权重位于存储库中的正确位置，为推理过程做好准备。

###  — 步骤 6 —
现在我们已经设置了环境并获得了模型权重，我们可以运行推理脚本了。在终端中执行以下命令：
```
python inference.py
```
这时，我们便可以坐下来观看 Replit 代码指示模型根据您的提示生成代码片段和有见地的解释。

![](https://s2.loli.net/2023/09/20/pCFTxXgcsJKiWdH.gif)

![](https://s2.loli.net/2023/09/20/R2ZxuWnt4hCmgfD.gif)

因此，继续，潜入Replit Code Instruct的世界，并在这个创新的AI模型的帮助下让您的创造力蓬勃发展。祝您编码愉快！


