---
title: 附录
date: 2023-04-26 22:04:21
tags: 统计学习方法
categories:
    - subjects
    - AI
top: 1
hidden:
---
&ensp;
<!-- more -->
## 1.1 heoffding inequality
先介绍一个引理马尔可夫不等式
### 马尔可夫不等式
对x非负随机变量，假设其期望存在，那么有a>0是的下面不等式成立：
$$
P(x\geq a)\leq{E(x)\over a}
$$
以下给出连续随机变量的证明，离散变量的证明类似，不多赘述。
证明：
$$
\begin{aligned}
E(x)&=\int_0^{+\infty}xf(x)dx\\
&\geq \int_a^{+\infty}xf(x)dx\\
&\geq a\int_a^{+\infty}f(x)dx\\
&=aP(x\geq a)
\end{aligned}
$$
马尔可夫不等式说明了尾部事件概率上限，但是只是适合大于0的随机变量，为了推广马尔可夫不等式，可以做出如下变换：
$$
\def\vep{\varepsilon}\\\\
P(e^x\geq e^a)\leq {E(e^x) \over e^a}\\
令\vep = e^a,则\vep>0,
P(e^x\geq \vep)\leq {E(e^x) \over \vep}
$$
现对随机变量x加两条限制，令E(x)=0,且有界，即$a_i\leq x\leq b_i$



