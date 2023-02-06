---
title: SVM 学习
date: 2023-02-2 9:00:00
tags: Machine Learning
math: true
---

# 支持向量机（Support Vector Machine）

> 最大间隔分类算法。在样本少时，预测效果好

## 1 线性模型

> 线性可分样本集（Linear Separable）；不可分就是（Non-linear...）

**按照以下7步进行分析**

### 1.1 哪一条直线最好？

提到最好，根据No Free Lunch Theorem，可知必须要有先验假设。

### 1.2 定义Performance Measure + 论证不同分法的性能

最好情况：d 取最大值(margin)

定义：d平行线碰到的向量为**支持向量**

*因为最终训练的模型只与支持向量相关，所以SVM在小样本表现很好*

### 1.3 定义训练数据及标签

$$(x_1,y_1),(x_2,y_2),(x_3,y_3),...,(x_n,y_n)$$

$x_n$ ：向量

$y_n$ ：标签 +1/-1 *方便将后面的两个约束条件写成约束方程。*


### 1.4 定义线性模型所做的事

- 线性模型：`二维 线` $\rightarrow$ ` 高维 超平面（Hyperplane）`

- $(\omega,b)$

    $\omega\rightarrow$ 向量；维度等于 $x$ 的维度。

    b：常数

- 线性方程： $W^TX+b=0$
    
    $W^T \rightarrow 常数$

*用机器学习的算法在这个模型的限定下求出* $(\omega,b)$

### 1.5 定义线性可分

一个训练集线性可分是指 ${(x_i,y_i)}_{i=1...N}$ $\exists$ $(\omega,b), st. 对\forall i=1...N$ 有

a) IF $y_i= +1$ ，则 $W^Tx_i+b \geq 0$
b) IF $y_i= -1$ ，则 $W^Tx_i+b < 0$

将a式与b式综合起来得出：

$$y_i[W^TX_i+b] \ge 0\tag1$$

线性可分便是使得公式(1)存在解。

### 1.6 优化问题

> 也是一个凸优化问题-二次规划

最小化（Minimize）： 限制条件（Subject to) 

$$\frac{1} {2}||W||^2 \quad st. \quad y_i[W^TX_i+b] \ge C \quad(i=1...N)$$

$\frac{1} {2}$ ：求导方便

$C$ ：常数，常取1，方便计算

**两个事实**

- 事实一： $W^Tx+b=0$ 与 $aW^tx+ab=0$ 是同一个平面。 $a \in R^+$ 若 $(\omega,b)$ 满足公式(1)，则 $(a\omega,ab)$ 也满足公式(1)

- 事实二：

    - 点到平面的距离公式

        平面： ${\omega}_1x+{\omega}_2y+b=0$ ，则 $(x_0,y_0)$ 到平面的距离：
        
        $$
        d = \frac{|{\omega}_1x_0+{\omega}_2y_0+b|} {\sqrt{ {\omega}_1^2+{\omega}_2^2} }
        $$

    - 同理，向量 $x_0$ 到超平面 $W^Tx+b=0$ 的距离

        $$
        d = \frac{|W^Tx_0+b|} {||W||}
        $$

        $$
        ||W||=\sqrt{ {\omega}_0^2+{\omega}_1^2+...}
        $$

所以，我们可以用a去缩放 $(\omega,b) \rightarrow (a\omega,ab)$ ，使得最终在支持向量 $x_0$ 上有 $|W^Tx_0+b|=1$

此时，支持向量与平面距离 $d = \frac{1} {||W||}$

那么，最小化 $||W||^2$ 就等于最大化 $d$

-  $||W||^2$ 方便求导

到这里问题转化为了凸优化-二次规划问题（Quadratic Programming）

> 凸优化-二次规划问题的特点：
> 1.目标函数（Object Function）二次项 2.限制条件二次项。
> 这类问题要么无解，要么只有一个极值，且局部极值即为全局极值，因此使用梯度下降求解即可。

## 2 非线性模型

> 大体思路是增加松弛变量，去高维空间找直线，理论涉及泛函

欠着，日后更。。。
