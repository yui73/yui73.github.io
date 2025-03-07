---
title: Latex的一些小技巧
date: 2025-03-07 15:51:00
tags: Dev
---

# Latex的一些小技巧

Latex在长论文排版时真的非常好用，学到了一些小技巧，下做简记。

## 1. 图片浮动问题

因为Latex是自动计算图片位置的，因此会出现图片位置大混乱的情况。有两个办法可以对这种情况进行调整。

**建议：整体论文定稿之后再进行图片位置或者大小调整。**

### 1.1 使用`H`强制图片插入在当前位置

```Latex
\begin{figure}[H]
    \centering
    \includegraphics[width=1\textwidth]{fig/yourfigure.png}
    \caption{图片}
    \label{fig:2}
\end{figure}
```
  
- 位置参数：
    - h：here，此刻的位置
    - t：top，置顶
    - b：buttom，置底
    - H：强制处于当前位置

- 宽度参数：
    - `width=1\textwidth`：宽度按照页面文字宽度的比例渲染，`0.5\textwidth`小数也是支持的🆗
    - `\label{fig:2}`：图片的引用路径，在正文使用`~\ref{fig:2}`进行引用，会自动进行正文的图注链接插入和图目录生成

### 1.2 直接在图片后面新起一页


```Latex
\begin{figure}[H]
    \centering
    \includegraphics[width=1\textwidth]{fig/yourfigure.png}
    \caption{图片}
    \label{fig:2}
\end{figure}

\newpage
```

使用`\newpage`指令，这样至少不会和下一章的内容混在一起。

## 2. 表格插入问题

### 2.1 快速构建表格

万能的GPT给的网址：[Tablesgenerator](https://www.tablesgenerator.com/)

可视化的表格编辑，真的很方便

- 支持直接黏贴数据
- 支持CSV导入
- 支持Latex代码导入和导出生成

### 2.2 行内数据换行

可以使用`\\`和`\newline`，两种命令。

位置参数、宽度参数和引用的使用同第一节图片插入。

```Latex
\begin{table}[htp]
	\centering % 表格居中
    % \resizebox{1\textwidth} % 设置表格缩放
	\vspace{5pt} % 大括号内设置表格与正文之间的间距
        \begin{tabular}{p{0.9\textwidth}}
            \hline
            \textbf{列标题} \\
            \hline
            First \newline Second\\
            \hline
        \end{tabular}
        \caption{表格} % 大括号内定义图片标题
        \label{tab:1} % 大括号内定义图片标签，用于正文引用
\end{table}
```

### 2.3 标题与内容的行距调整

在表格代码前插入`\renewcommand{\arraystretch}{1.5}`，这行命令将表格行与行间距扩大到原先的1.5倍。

{% note info %}
值得注意的是：使用`\\`命令进行行内换行的时候，会受到`\renewcommand{\arraystretch}{1.5}`命令的影响，因此行内换行的间距也会变大，而使用`\newline`则不会。
{% endnote %}

```Latex
\renewcommand{\arraystretch}{1.5}
\begin{table}[htp]
	\centering % 表格居中
    % \resizebox{1\textwidth} % 设置表格缩放
	\vspace{5pt} % 大括号内设置表格与正文之间的间距
        \begin{tabular}{p{0.9\textwidth}}
            \hline
            \textbf{列标题} \\
            \hline
          First \newline Second\\
          \hline
        \end{tabular}
        \caption{表格} % 大括号内定义图片标题
        \label{tab:1} % 大括号内定义图片标签，用于正文引用
\end{table}

```

## 3. 论文渲染版本控制

> 这段内容是从Overleaf上的华师大硕/博论文模板中复制📋到的。由于我一开始使用了导师提供的模板，但很眼馋这个模板的功能，因此将这个功能融合到自己的模板里，此处做一个学习记录。

{% note info %}
**主要功能：为了实现提交不同用处（盲审/学术不端检测/终稿）的论文时，可以快速渲染出符合要求的论文内容，不用手动去每个地方做修改。**

*论文要修改非常多版本，每次都手动修改太麻烦，也非常容易出错。*
{% endnote %}

### 3.1 定义相关变量

```Latex
\def \docstyle {normal} % normal,tmlc,anonymous

\def \dd {anonymous}
\def \de {tmlc}
\newif\ifnotanonymous\notanonymoustrue %定义是否开启盲审，默认不开启
\newif\iftmlc
```

- `\docstyle`：文档类型的控制变量
- `\dd`：字符串变量，用于判断是否是`anonymous`类型的变量（控制判断的逻辑分支）
- `\de`：字符串变量，用于判断是否是`tmlc`类型的变量（控制判断的逻辑分支）
- `\ifnotanonymous`：布尔值变量，`\notanonymoustrue`默认为`true`
- `\iftmlc`：布尔值变量，未定义初值，默认为`false`

### 3.2 判断文档类型（修改控制变量）

```Latex
\ifx \docstyle \dd
    \notanonymousfalse % 开启该行则为盲审版本，注释该行则为非盲审版本（即查重或终稿版本）
\else
    \ifx \docstyle \de
        \tmlctrue % 开启该行则为查重版本，注释该行则为终稿版本（仅在上一行在注释状态下有效）
    \fi
\fi
```

本段逻辑（伪代码）:

```
if \docstyle == \dd(anonymous):
    \notanonymous = false
else:
    if \docstyle == \de(tmlc):
        \tmlc = true
```
### 3.3 控制页面渲染

- 盲审：原创性✅，名单❌，隐藏致谢
- 学术不端检测：原创性❌，名单❌，致谢❌
- 终稿：都要

#### 3.3.1 控制页面
```Latex
\ifnotanonymous %非盲审，也就是查重版，不显示原创性+名单；终版全要
    \iftmlc
    \else
        \input A3-COPYRIGHT.tex  %注释掉部分
        \cleardoublepage
        \input A4-MEMBERLIST.tex %注释掉部分
        \cleardoublepage
    \fi
\else %盲审仅显示原创性，不要名单
    \input A3-COPYRIGHT.tex  %注释掉部分
    \cleardoublepage %查重不显示
\fi

%%%%%
%中间省略
%%%%%

\ifnotanonymous %非盲审，查重：不要致谢，终稿要
    \iftmlc
    \else
        \phantomsection
        \input D2-ACHNOWLEDGEMENT.tex
        \addcontentsline{toc}{chapter}{致谢}
        \cleardoublepage %查重不显示
    \fi
\else %盲审，需要短的致谢
    \phantomsection
    \input D2-ACHNOWLEDGEMENT-TMLC.tex
    \addcontentsline{toc}{chapter}{致谢}
    \cleardoublepage
\fi
```

#### 3.3.2 控制页面内部内容渲染

使用下述语句控制名字和敏感信息的隐藏（只有匿名稿需要隐藏）

```Latex
\ifnotanonymous Your Name \else *** \fi
```