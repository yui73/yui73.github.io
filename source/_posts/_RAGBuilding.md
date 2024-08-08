---
title: RAG系统搭建
date: 2024-08-02 16:45:00
tags: 论文
---

# RAG系统搭建

## 0 RAG应用的组成

一个RAG应用可以分成下列5个部分组成：文档加载（Document Loading）、文本分割（Splitting）、存储（Storage）、检索（Retrieval）和输出（Output to LLM）。

本文计划使用`Langchain`框架搭建系统。

## 1 从加载到存储

### 1.1 文档加载（Document Loading）

`Langchain`框架集成了丰富的文档加载器。

文档链接：[Langchain Community](https://api.python.langchain.com/en/latest/community_api_reference.html)

### 1.2 文本分割（Splitting）

#### 1.2.1 文本分割概念理解

对于LLM应用，将长文档切割成小块，以便放入上下文窗口中是非常必要的。此处，事实上存在许多隐藏的复杂性。比如说，理想状态下，我们希望能将`语义相关`的文本片段放在一起，而`语义相关`取决于文本的类型。

从高层级来看，文本分割的工作流程如下：

- 将文本**分割**成语义上有意义的小块（通常是句子）。
- 将这些小块组合成一个大块，直到达到一定的大小（**用某个函数来衡量**）。
- 一旦达到一定大小，就将该语块作为自己的文本，然后创建有一定*重叠*的新语块（以保持语块之间的上下文）。

这就意味着有两个不同的轴可以用来定制文本分割器：

- 如何分割文本
- 如何测量文本块大小

文档链接：[Lanchain text-splitters](https://python.langchain.com/v0.2/docs/how_to/#text-splitters)

#### 1.2.2 文本分割模式选择

##### a. 按照语义相似度分割文本

参考文档：[Langchain semantic-chunker](https://python.langchain.com/v0.2/docs/how_to/semantic-chunker/)  |  [5_Levels_Of_Text_Splitting](https://github.com/FullStackRetrieval-com/RetrievalTutorials/blob/main/tutorials/LevelsOfTextSplitting/5_Levels_Of_Text_Splitting.ipynb)

原理理解稍后补...

1. 安装依赖

```sh
conda install langchain-experimental langchain-openai
```

2. 加载内容

```python
# This is a long document we can split up.
with open("state_of_the_union.txt") as f:
    state_of_the_union = f.read()
```

3. 创建Text Splitter

    **a. 连接Embedding模型**
    - OpenAI
    ```python
    from langchain_experimental.text_splitter import SemanticChunker
    from langchain_openai.embeddings import OpenAIEmbeddings

    text_splitter = SemanticChunker(OpenAIEmbeddings())
    ```

    - AzureAI

    ```python
    import os

    os.environ["OPENAI_API_TYPE"] = "azure"
    os.environ["OPENAI_API_BASE"] = "https://<your-endpoint.openai.azure.com/"
    os.environ["OPENAI_API_KEY"] = "your AzureOpenAI key"
    os.environ["OPENAI_API_VERSION"] = "2023-05-15"
    os.environ["OPENAI_PROXY"] = "http://your-corporate-proxy:8080"

    from langchain_community.embeddings.openai import OpenAIEmbeddings
    embeddings = OpenAIEmbeddings(
        deployment="your-embeddings-deployment-name",
        model="your-embeddings-model-name",
        openai_api_base="https://your-endpoint.openai.azure.com/",
        openai_api_type="azure",
    )
    ```

    **b. 创建text_splitter**

    ```python
    text_splitter = SemanticChunker(OpenAIEmbeddings())

    docs = text_splitter.create_documents(data)
    print(docs[0].page_content)
    ```

    **c. 确定断点BreakPoint**
    具体理解稍后补上...
    1. Percentile
    2. Standard Deviation
    3. Interquartile
    4. Gradient

## 99 参考内容

1. [CSDN](https://blog.csdn.net/python12222_/article/details/139626405)