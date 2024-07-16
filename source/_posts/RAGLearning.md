---
title: RAG系统理论学习
date: 2024-07-15 15:00:00
tags: 论文
---

# RAG系统理论学习

> 对于RAG应用的检索系统搭建的理论与工程部分进行学习。
> 参考文献：https://arxiv.org/abs/2312.10997

## 0 引入

当LLM遇到超出其训练数据或遇到需要查询当前数据时，就会产生`hallucinations`幻觉现象。现在通过RAG技术，在外部知识库中通过计算语义相似度`semantic similarity calculation`，检索相关文档块，降低幻觉产生。

#### 阶段特征

- 与Transformer架构的融合：通过预训练模型与额外知识的合并，增强语言模型。**这个阶段的目的是细化预训练技术。**
- 与LLM架构的融合：ChatGPT的出现展现了LLM的强大ICL能力。此时，RAG技术转向为LLM在推理阶段的信息提供。如今开始偏向结合LLM的微调技术。

## 1 RAG的主要概念和当前范式

> RAG最典型的应用是与ChatGPT结合，补充ChatGPT在训练时与当前世界的信息差距，将外部数据中搜索到的信息+用户提问形成一个全面的Prompt，促使LLM生成更好的答案。

### 1.1 RAG技术的3个阶段

1. Naive RAG

RAG技术的早期阶段，高光时刻是在ChatGPT被广泛采用的时候。这个时候的框架称作`Retrieve-Read`框架,包括了`索引`、`检索`和`生成`。

整体流程：

![Naive RAG](<NAIVE RAG.png>)

2. Advanced RAG
3. Modular RAG


## 2 核心组成——检索、生成和增强
## 3 检索优化——索引、查询和嵌入优化。
## 4 三个增强过程
## 5 RAG 的下游任务和评估系统
## 6 不足之处

