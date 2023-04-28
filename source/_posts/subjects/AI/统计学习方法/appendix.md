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
\tag{1.1}
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
\tag{1.2}
$$

此外还需要介绍一个不等式Jensen不等式，若一个函数为凸函数，则有$t\in (0,1)$和两个点使得不等式成立；
$$
f(tx_1+(1-t)x_2)\leq tf(x_1)+(1-t)f(x_2)
$$
Jensen不等式的理解就是指在两个点上的直线上取值总是大于在函数上取值。

现对随机变量x加两条限制，令E(x)=0,且有界，即$a_i\leq x\leq b_i$。
那么则有$\forall t\in(0,1)$（注：t参数是为了变换指数的底数）使得以下不等式成立：
$$
\def\vep{\varepsilon}\\\\
\begin{aligned}
    P(e^{tx}\geq e^{t\vep})&\leq e^{-t\vep}E(e^{tx})\\
\end{aligned}
\tag{1.3}
$$
因为$x\in(a_i,b_i)$，此时令$x=\alpha a_i+(1-\alpha)b_i$，则$\displaystyle\alpha ={ {x-b_i}\over{a_i-b_i}}$可得：
$$
\begin{aligned}
    E(e^{tx})&=E(e^{\alpha ta_i+(1-\alpha)tb_i})\\
    &\leq E(\alpha e^{ta_i}+(1-\alpha)e^{tb_i})\\
    &=E({ {x-b_i}\over{a_i-b_i}}e^{ta_i}+{ {a_i-x}\over{a_i-b_i}}e^{tb_i})\\
    \because &E(x)=0\\
    &={ {a_i}\over{a_i-b_i}}e^{tb_i}-{ {b_i}\over{a_i-b_i}}e^{ta_i}
\end{aligned}
$$
接着整合式子:
$$
\displaystyle
\begin{aligned}
    { {a_i}\over{a_i-b_i}}e^{tb_i}-{ {b_i}\over{a_i-b_i}}e^{ta_i}&={b_i\over {a_i-b_i}}e^{ta_i}({a_i\over b_i}e^{t(b_i-a_i)}-1)\\
    &=e^{\ln{b_i\over{a_i-b_i}}+ta_i+\ln{(e^{t(b_i-a_i)}-1)}}\\
    &=e^{ta_i+\ln{({a_i\over{a_i-b_i}} e^{t(b_i-a_i)}-{b_i\over{a_i-b_i}})}}\\
    令u=t(b_i-a_i)&,\gamma={b_i\over{b_i-a_i}}\\
    &=e^{(\gamma-1)u+\ln{[(1-\gamma)e^u +\gamma]}}\\
    &=e^{g(u)}
\end{aligned}
$$

由上式知$g(u)=(\gamma-1)u+\ln{[(1-\gamma)e^u +\gamma]}$由泰特定理可知当$g(u)$为光滑函数时，有$\exist \xi\in(0,u)$使得$g(u)=g(0)+g'(0)u+{1\over2}g''(\xi)u^2$，已知$\displaystyle g(0)=0,g'(0)=0,g''(u)={ {\gamma(1-\gamma)e^u}\over{[(1-\gamma)e^u+\gamma]^2}}$。由均值不等式$\displaystyle g''(u)\leq{1\over4}$
因此，$g(u)\leq {1\over8}u^2={t^2\over8}(b_i-a_i)^2$。

结合（1.3）得
$$
\begin{aligned}
    P(e^{tx}\geq e^{t\varepsilon})&\leq e^{-t\varepsilon}E(e^{tx})\leq e^{-t\varepsilon}e^{g(x)}\leq e^{-t\varepsilon}\exp({t^2\over8}{(b_i-a_i)^2})
\end{aligned}
\tag{1.4}
$$
若有$X_1,X_2,\cdots,X_N$独立分布的随机变量由式（1.2）得：
$$
\begin{aligned}
    P(\sum_{i=1}^NX_i\geq \varepsilon)&=P(e^{t\sum_{i=1}^N{X_i}}\geq e^{t\varepsilon})\\
    &\leq e^{-t\varepsilon}E(e^{t\sum_{i=1}^N{X_i}})\\
    &=e^{-t\varepsilon}\prod_{i=1}^N E(tX_i)\\
    由（1.4）:\\
    &\leq e^{-t\varepsilon}\prod_{i=1}^N \exp({t^2\over8}{(b_i-a_i)^2})
\end{aligned}
$$
由此得证:
$$
P(\sum_{i=1}^NX_i\geq \varepsilon)\leq e^{-t\varepsilon}\prod_{i=1}^N \exp({t^2\over8}{(b_i-a_i)^2})
$$



