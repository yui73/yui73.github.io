---
title: Prompt Engineering 人工构建Prompt
date: 2023-11-08 14:00:00
tags: Prompt Learning
---

# Prompt Engineering 人工构建Prompt

> 学习链接：[微信公众号](https://mp.weixin.qq.com/s?__biz=Mzk0NzMwNjU5Nw==&mid=2247483914&idx=1&sn=cf8aab9a1ff04dc4fec3528708f02f26&chksm=c379ab00f40e22161d15beeef01fd380910c188f9f70d8e7e849b7a7c8d233abb7e72fa87497&scene=21#wechat_redirect)

人工构建Prompt适合Zero-Shot的情况，可以浅尝效果。人工构建Prompt相当泛用，因此学习一些它的应用场景。

## 1 LAMA

> 源码地址：[Github](https://github.com/facebookresearch/LAMA)

LAMA数据集，是一个对预训练语言模型的探针，使用Prompt来分析预训练语言模型掌握了哪些知识。

## 2 GPT3

GPT3 是通过大量语料训练得到的，对于大量下游任务，无需任何参数更新和微调。对于特定的下游任务可以实现Few-Shot，论文中的Prompt都是通过人工构造。

{% note info %}
**后续细读其人工构建Prompt和评判模型效果的部分并复现**
{% endnote %}

论文：[(2020) Language models are few-shot learners](https://arxiv.org/pdf/2005.14165.pdf)

## 3 PET(Pattern Exploiting Training)

学习链接：[知乎](https://zhuanlan.zhihu.com/p/644369467)

PET是一种半监督训练方法，它的目标有两个：

1. 将任务目标以自然语言的模式加入训练数据中

2. 对无标签数据进行样本挖掘以获取有标签数据
