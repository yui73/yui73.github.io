---
title: 扩散模型学习
date: 2023-09-16 21:33:00
tags: Machine Learnig
---

# 扩散模型

> 看了篇论文，里面提到了扩散模型，前段时间听师兄说老师最近对扩散模型很感兴趣，所以我就找了个视频学习了一下

学习视频：[B站](https://www.bilibili.com/video/BV1tz4y1h7q1/?share_source=copy_web&vd_source=cb49ee774cbb55d416f80adbd9365da5)

学习代码：[Github](https://github.com/wangjia184/diffusion_model)

非常感谢这位UP，讲得非常深入浅出。

## 1 扩散模型

简单来说就是这个方程：

$$ x_t = \sqrt{1-β_t}\times x_{t-1} + \sqrt{β_t}\times ϵ_{t}  \tag1$$

其中ϵ是噪声，x是图像，上述方程描述了噪声在图像当中扩散的状态。$ϵ_t\sim N(0,1)$，每次重新生成。

同时 $$ 0 < β_1 < β_2 < β_3 < \dots < β_T < 1 $$ 表示噪声扩散速度越来越快。

有看到说， $(\sqrt{\beta})^2+(\sqrt{1-\beta})^2 = 1$ 是为了保证能量守恒。

引入 $a_t = 1 - β_t$，可以得到：

$$ x_t = \sqrt{a_{t}}\times x_{t-1} +  \sqrt{1-a_t} \times ϵ_{t} $$

## 2 前向传播

我们知道前向传播的过程是：$$ x_0 \overset{q(x_1 | x_0)}{\rightarrow} x_1 \overset{q(x_2 | x_1)}{\rightarrow} x_2 \rightarrow \dots  \rightarrow x_{T-1} \overset{q(x_{t} | x_{t-1})}{\rightarrow} x_T $$

所以首先探讨如何避免多次迭代？

### 2.1 避免多次迭代

{% note info %}
**我们从 $x_{t-2}$ 与 $x_{t}$ 的关系开始讨论：**
{% endnote %}

我们根据方程(1)可得：

$$ x_{t-1} = \sqrt{a_{t-1}}\times x_{t-2} +  \sqrt{1-a_{t-1}} \times ϵ_{t-1}$$ 

$$ x_t = \sqrt{a_{t}}\times x_{t-1} +  \sqrt{1-a_t} \times ϵ_{t} $$

因此，把$x_{t-1}$代入，不难得出：

$$ x_t = \sqrt{a_{t}a_{t-1}}\times x_{t-2} +  \sqrt{a_{t}(1-a_{t-1})} ϵ_{t-1} +  \sqrt{1-a_t} \times ϵ_t \tag2$$

我们已知：$ϵ\sim N(0,1)$

所以此时相当于，将原先丢一个骰子的问题，转化成同时丢俩个骰子的问题，也就是叠加概率分布。此处用卷积方法对概率求总和。我们知道正态分布的卷积仍然符合正态分布，即：

$$N(\mu_1,\sigma_1^2)+N(\mu_2,\sigma_2^2)=N(\mu_1+\mu_2,\sigma_1^2+\sigma_2^2) \tag3$$

此时我们便能知道，对于方程(2)：

$$ϵ_t \sim N(\mu_t,\sigma_t^2)$$

$$\sqrt{1-a_t} \times ϵ_t \sim N(\sqrt{1-a_t}\mu_t , (\sqrt{1-a_t})^2\sigma_t^2)$$

$$\sqrt{a_{t}(1-a_{t-1})} ϵ_{t-1} \sim N(\sqrt{a_{t}(1-a_{t-1})}\mu_t , (\sqrt{a_{t}(1-a_{t-1})})^2\sigma_{t-1}^2)$$

因为$ϵ_t\sim N(0,1)$

所以：

$$\sqrt{1-a_t} \times ϵ_t \sim N(0 , 1-a_t)$$

$$\sqrt{a_{t}(1-a_{t-1})} ϵ_{t-1} \sim N(0 , a_{t}(1-a_{t-1}))$$

将上述两个结果代入公式(3)：

$$\sqrt{a_{t}(1-a_{t-1})} ϵ_{t-1} +  \sqrt{1-a_t} \times ϵ_t \sim N(0,1-a_{t}a_{t-1})$$

使用重参数化，产生新采样ϵ，将方程(2)转化为：

$$ x_t = \sqrt{a_{t}a_{t-1}}\times x_{t-2} +  \sqrt{1-a_{t}a_{t-1}} \times ϵ \tag4$$

{% note info %}
**按照这个思路，我们进一步探讨 $x_{t-3}$ 与 $x_{t}$ 的关系：**
{% endnote %}

$$ x_{t-2} = \sqrt{a_{t-2}}\times x_{t-3} +  \sqrt{1-a_{t-2}} \times ϵ_{t-2} $$

$$ x_t = \sqrt{a_{t}a_{t-1}}\times x_{t-2} +  \sqrt{1-a_{t}a_{t-1}} \times ϵ $$

不难得出：

$$ x_t = \sqrt{a_{t}a_{t-1}a_{t-2}}\times x_{t-3}  +  \sqrt{a_{t}a_{t-1}-a_{t}a_{t-1}a_{t-2}} ϵ_{t-2} +  \sqrt{1-a_{t}a_{t-1}}\times ϵ $$

其中：

$$ \sqrt{1-a_{t}a_{t-1}}\times ϵ \sim N(0,1-a_{t}a_{t-1})$$

$$ \sqrt{a_{t}a_{t-1}-a_{t}a_{t-1}a_{t-2}}\times ϵ_{t-2} \sim N(0,a_{t}a_{t-1}-a_{t}a_{t-1}a_{t-2})$$

即：

$$ x_t = \sqrt{a_{t}a_{t-1}a_{t-2}}\times x_{t-3} +  \sqrt{1-a_{t}a_{t-1}a_{t-2}} \times ϵ $$

由此，我们不难看出$x_t$与$x_{t-k}$的关系（可以使用数学归纳法证明）：

$$x_t = \sqrt{a_{t}a_{t-1}a_{t-2}a_{t-3}...a_{t-(k-2)}a_{t-(k-1)}}\times x_{t-k} +  \sqrt{1-a_{t}a_{t-1}a_{t-2}a_{t-3}...a_{t-(k-2)}a_{t-(k-1)}}\times ϵ$$

$x_t$与$x_0$的关系：

$$x_t = \sqrt{a_{t}a_{t-1}a_{t-2}a_{t-3}...a_{2}a_{1}}\times x_{0} +  \sqrt{1-a_{t}a_{t-1}a_{t-2}a_{t-3}...a_{2}a_{1}}\times ϵ$$

**我们引入：**

$$\bar{a}_{t} := a_{t}a_{t-1}a_{t-2}a_{t-3}...a_{2}a_{1}$$

得出前向传播的方程：

$$x_{t} = \sqrt{\bar{a}_t}\times x_0+ \sqrt{1-\bar{a}_t}\times ϵ , ϵ \sim N(0,I) \tag 5$$

## 3 反向过程 $p$

### 3.1 贝叶斯公式

$$P(A|B) = \frac{ P(B|A)P(A) }{ P(B) }$$

其中：$P(A)$ 称为Prior - 先验概率；$P(A|B)$ 称为Posterior - 后验概率；$P(B)$ 称为Evidence - 证据；$P(B|A)$称为似然 - likelihood，表示B对A的证据强度。

### 3.2 马尔可夫链

>这里摘抄一下，马尔可夫链（Markov chain），又称离散时间马尔可夫链（discrete-time Markov chain），为状态空间中经过从一个状态到另一个状态的转换的随机过程。该过程要求具备“无记忆”的性质：下一状态的概率分布只能由当前状态决定，在时间序列中它前面的事件均与之无关。这种特定类型的“无记忆性”称作马尔可夫性质。在马尔可夫链的每一步，系统根据概率分布，可以从一个状态变到另一个状态，也可以保持当前状态。状态的改变叫做转移，与不同的状态改变相关的概率叫做转移概率。随机漫步就是马尔可夫链的例子。随机漫步中每一步的状态是在图形中的点，每一步可以移动到任何一个相邻的点，在这里移动到每一个点的概率都是相同的（无论之前漫步路径是如何的）。

简单来说，马尔可夫链某一时刻状态转移的概率只依赖于它的前一个状态，具有`无记忆性`。

其数学表达为： $P(x_t|x_{t-1},x_{t-2}\dots,x_{0}) = P(x_t|x_{t-1}) $

掌握上述两个定理后，我们开始推导反向过程 $p$ (Reverse Process $p$)

### 3.3 Reverse Process $p$

我们已知：

$$\bar{a}_{t} := a_{t}a_{t-1}a_{t-2}a_{t-3}...a_{2}a_{1}$$

$$x_{t} = \sqrt{\bar{a}_t}\times x_0+ \sqrt{1-\bar{a}_t}\times ϵ , ϵ \sim N(0,I) $$

从前向传播的方程我们也知道：

$$ x_t = \sqrt{a_{t}}\times x_{t-1} +  \sqrt{1-a_t} \times ϵ_{t} $$

因此在反向过程中，我们需要从 $x_{t}$ 求 $x_{t-1}$，根据贝叶斯公式：

$$ p(x_{t-1}|x_{t}) = \frac{ q(x_{t}|x_{t-1})\times q(x_{t-1})}{q(x_{t})} $$

为了严谨，我们都加上条件 $x_{0}$：

$$ p(x_{t-1}|x_{t},x_{0}) = \frac{ q(x_{t}|x_{t-1},x_{0})\times q(x_{t-1}|x_0)}{q(x_{t}|x_0)} \tag6$$

*（实际跟据马尔科夫链的无记忆性，加不加是一样的）*

我们已知，正态分布的概率密度函数为：

$$ f(x) = \frac{1}{\sqrt{2\pi } \alpha} e^{\left (  -\frac{1}{2}\frac{(x_{t}-\mu)^2}{2\alpha^2}   \right ) } $$

在此，整理所有的条件：

$$ϵ\sim N(0,1)$$

即，可得：

$$ x_{t} = \sqrt{\bar{a}_t}\times x_0+ \sqrt{1-\bar{a}_t}\times ϵ \sim N(\sqrt{\bar{a}_t}\times x_0,1-\bar{a}_t) $$


$$ P(x_{t}|x_{0}) = \frac{1}{\sqrt{2\pi } \sqrt{1-\bar{a}_{t}}} e^{\left (  -\frac{1}{2}\frac{(x_{t}-\sqrt{\bar{a}_{t}}x_0)^2}{1-\bar{a}_{t}}   \right ) }  \tag7$$

即，可得：

$$ x_t = \sqrt{a_{t}}\times x_{t-1} +  \sqrt{1-a_t} \times ϵ_{t} \sim N(a_{t}\times x_{t-1},1-a_t) $$

$$ P(x_{t}|x_{t-1},x_{0}) = \frac{1}{\sqrt{2\pi } \sqrt{1-a_{t}}} e^{\left (  -\frac{1}{2}\frac{(x_{t}-\sqrt{a_t}x_{t-1})^2}{1-a_{t}}   \right ) } \tag8$$

代入 $x_{t-1}$ 与 $x_0$ 的关系前向传播方程
即，可得：

$$ x_{t-1} = \sqrt{\bar{a}_{t-1}}\times x_0+ \sqrt{1-\bar{a}_{t-1}}\times ϵ \sim N(\sqrt{\bar{a}_{t-1}}\times x_0,1-\bar{a}_{t-1}) $$

$$ P(x_{t-1}|x_{0}) = \frac{1}{\sqrt{2\pi } \sqrt{1-\bar{a}_{t-1}}} e^{\left (  -\frac{1}{2}\frac{(x_{t-1}-\sqrt{\bar{a}_{t-1}}x_0)^2}{1-\bar{a}_{t-1}}   \right ) } \tag9$$

将公式(7)(8)(9)代入公式(6)，得出方程：

$$ \frac{ q(x_{t}|x_{t-1},x_{0})\times q(x_{t-1}|x_0)}{q(x_{t}|x_0)} $$

$$= \left [
  \frac{1}{\sqrt{2\pi} \sqrt{1-a_{t}}} e^{\left (  -\frac{1}{2}\frac{(x_{t}-\sqrt{a_t}x_{t-1})^2}{1-a_{t}}   \right ) } 
\right ] * 
\left [ 
\frac{1}{\sqrt{2\pi} \sqrt{1-\bar{a}_{t-1}}} e^{\left (  -\frac{1}{2}\frac{(x_{t-1}-\sqrt{\bar{a}_{t-1}}x_0)^2}{1-\bar{a}_{t-1}}   \right ) }  
\right ] \div
\left [ 
  \frac{1}{\sqrt{2\pi} \sqrt{1-\bar{a}_{t}}} e^{\left (  -\frac{1}{2}\frac{(x_{t}-\sqrt{\bar{a}_{t}}x_0)^2}{1-\bar{a}_{t}}   \right ) }
\right ]  $$

*(我算到这里就不会解了XD，后面看别人的推导了)*

最后，可得：

$$ \frac{1}{\sqrt{2\pi} \left ( {\frac{ \sqrt{1-a_t} \sqrt{1-\bar{a}_{t-1}} } {\sqrt{1-\bar{a}_{t}}}}  \right ) }  
exp \left[
-\frac{1}{2}
\frac{
  \left(
    x_{t-1} - \left(
      {\frac{\sqrt{a_t}(1-\bar{a}_{t-1})}{1-\bar{a}_t}x_t
      +
      \frac{\sqrt{\bar{a}_{t-1}}(1-a_t)}{1-\bar{a}_t}x_0} 
      \right)
  \right) ^2
} {   \left( {\frac{ \sqrt{1-a_t} \sqrt{1-\bar{a}_{t-1}} } {\sqrt{1-\bar{a}_{t}}}}  \right)^2 }
\right] $$

由于 $x_0$ 为我们需要的结果，因此：

根据 $x_{t} = \sqrt{\bar{a}_t}\times x_0+ \sqrt{1-\bar{a}_t}\times ϵ$, $x_0 = \frac{x_t - \sqrt{1-\bar{a}_t}\times ϵ}{\sqrt{\bar{a}_t}}$ ，替换 $x_0$ ，可得


$$ p(x_{t-1}|x_{t}) \sim N\left( 
      {\frac{\sqrt{a_t}(1-\bar{a}_{t-1})}{1-\bar{a}_t}x_t
      +
      \frac{\sqrt{\bar{a}_{t-1}}(1-a_t)}{1-\bar{a}_t}\times \frac{x_t - \sqrt{1-\bar{a}_t}\times ϵ}{\sqrt{\bar{a}_t}} } ,
       {\frac{ \beta_{t} (1-\bar{a}_{t-1}) } { 1-\bar{a}_{t}}} 
 \right) \tag{10}$$

**由此，我们可以利用公式(10)推导出来的关系，由 $x_T$ 不断反向推导到 $x_0$**

### 3.4 含义解释

我们理解含义为：任意时刻 $x_{t}$ 的图像，都是由 $x_{0}$ 时刻的图像直接加噪而来的。因此，只要知道ϵ，便可求出上一时刻$x_{t-1}$的概率分布。

此处，训练神经网络用于预测  $x_{t}$ 时刻的图像相对于  $x_{0}$ 时刻的图像加入的噪声ϵ，然后得到 $x_{t-1}$ 时刻图像的概率分布，用此概率分布进行随机采样，便可得到 $x_{t-1}$ 时刻图像，反复迭代上述过程，便可获得 $x_{0}$ 时刻的图像


我们已知：

$$\bar{a}_{t} := a_{t}a_{t-1}a_{t-2}a_{t-3}...a_{2}a_{1}$$

$$x_{t} = \sqrt{\bar{a}_t}\times x_0+ \sqrt{1-\bar{a}_t}\times ϵ , ϵ \sim N(0,I) $$

$x_{T}$ 时刻 $\bar{a}$ 接近于0，因此 $x_T \approx ϵ$，即 $x_T$ 的图像近似于标准正态分布，即为任意一张标准正态分布噪声图像。

也就是说，任意一张标准正态分布噪声图像都是由某张 $x_0$ 图像加噪声得来的，因此对标准正态分布的图像进行随机采样就可获得 $x_T$ 时刻图像。

此时，神经网络接受参数 $x_t$ 和 $t$ ， $x_t$ 用于预测噪声ϵ， $t$ 用于学习图像在加入噪声序列中的位置。

从 $T$ 到 $0$ 时刻，上一时刻的正态分布标准差越来越接近0，也就是说上一时刻图像越来越靠近均值，越来越确定。

**上述为DDPM模型反向过程生成图片的原理。**

> 感想：数学真奇妙。从标准正态分布噪声图像，通过不同噪声ϵ，便可反向推导出不同的 $x_0$ 时刻图像。