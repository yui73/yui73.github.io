---
title: Prompt Engineering
date: 2023-11-08 14:00:00
tags: Prompt Learning
---

# Prompt Engineering学习

> 如何利用好大语言模型，就需要给模型合适的Prompt，引导它给出合适的答案，我认为是模型微调的一种新方法。本文是自用的一个学习笔记，方便后续回顾。

学习链接：[微信公众号](https://mp.weixin.qq.com/s?__biz=Mzk0NzMwNjU5Nw==&mid=2247483914&idx=1&sn=cf8aab9a1ff04dc4fec3528708f02f26&chksm=c379ab00f40e22161d15beeef01fd380910c188f9f70d8e7e849b7a7c8d233abb7e72fa87497&scene=21#wechat_redirect)

## 0 NLP发展史

- 阶段一：feature engineering

这个阶段的模型十分依赖前期的特征工程，往往需要人工去手动提取和筛选定义特征，这个阶段特征质量会影响最后的模型效果。

- 阶段二：architecture engineering

这个阶段大家的目标是如何减少模型对特征的依赖，希望模型可以通过好的神经网络设计让模型自己学习有用的特征。

*上述两个阶段都为监督学习阶段。*

- 阶段三：objective engineering

这个阶段就是熟知的预训练时代，如何设计合理的预训练和微调的目标函数最为重要，对最后的结果影响深远，此时，已经进入无监督学习，极大的降低了对监督语料（特征）的依赖。

- 阶段四：prompt engineering

这个阶段就进入了我们目前的时代，这个阶段依旧会进行预训练，不同之处是用合适特定的Prompt的去重构下游任务并且管控模型行为，以实现Zero Shot或者Few Shot。因此这个阶段的关键任务是如何设计一个合理有效的Prompt。

{% note info %}
拓展知识-Zero Shot/Few Shot：
目前有监督学习的模型在很多问题上已经达到或者超过人类水平，然而有监督学习的性能依赖于喂给它的特征，因此它对于训练时没见过的类别具有局限性。
Zero Shot/Few Shot希望模型能具有人类的推理能力，像人类推理认识在训练时没见过的新事物。这样可以实现对于小样本或者零样本类别的识别。
{% endnote %}

## 1 Prompt Learning

传统监督学习任务：

$$ x {\xrightarrow{P(x)}} y$$

Prompt Learning：

$$ x {\xrightarrow{f_{prompt}(x)}} x' {\xrightarrow{语言模型}} x {\rightarrow} y$$

从上述对比，不难看出Prompt Learning主要分为三个部分：

1. Prompt Addition：定义$f_{prompt}(x)$，将原始的$x$转化为$x'$，$x'$上的空槽会直接决定最后的预测结果。

2. Answer Search：指对所有可能的候选槽值进行搜索，然后选择最合适的槽值。用$Z$来代表可能结果集，生成任务中，$Z$包含所有token；分类任务中，$Z$包含特定分类任务的相关token。

3. Answer mapping：当生成任务时，上一步已获得最终结果；分类任务时，还需要根据槽值，将结果归到对应的类别中。

## 2 影响Prompt Learning效果的关键

- 选择合适的Prompt模板，设计合适的模板函数，也就是Prompt Engineering。

- 预训练语言模型的选择。

- Answer Engineering，答案候选集和答案到最终结果映射的构建。

- Expanding the paradigm：拓展Prompt Learning，包括Multi Prompt。

- Training strategy，选择训练策略。

## 3 Prompt Engineering

学习链接：[知乎](https://zhuanlan.zhihu.com/p/488279606)

> 后续细学

- prefix prompt：在前缀的基础上填后续文本

- cloze prompt：在句子中间填空

其他分类方法：人工、自动、隐空间。连续、离散。

## 4 Multi-prompt Learning

多Prompt的类型如下：

- Ensemble：单个Prompt并行，最后通过加权和投票汇总结果。

- Augmentation：增强，通过找一个和$x'$接近的case一起输入，通过case判断$x'$的预测结果是否正确。

- **Composition：** 将多个Prompt函数融合到一起，每个Prompt负责一个子任务，同时进行多个子任务的预测，例如同时进行：关系抽取，实体识别，实体间关系判断。

- Decomposition：适用于有多个预测值的任务，将原始任务拆成多个子任务，每个子任务之间彼此隔离，子任务用单个Prompt去处理。

## 5 Training Strategy

根据语言模型选择训练策略，见下图。

![训练策略](https://mmbiz.qpic.cn/mmbiz_png/86qxNQYXKpsUR7A8n372cFXicXfdXdEWibhgJLmVVoFlyaSpTLKUqtdLCCOlsakOVgxc6cPLdbYafFhE3l7eKgog/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)
