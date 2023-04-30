---
title: 快速傅里叶变换与多项式乘法
date: 2023-04-30 11:52:10
tags: fft
categories: 
    - technology
    - knowledge
top:
hidden:
---
&ensp;
<!-- more -->

# 前言
多项式的乘法的一般计算方法是先对各个分量进行连续卷积，得到结果的各个分量。而傅里叶变换可以将卷积变成相乘，快速傅里叶变换则是利用多项式的奇偶分量对其进行分治处理。大大地提升了变换的速度。

# 预处理
对于一个多项式$a_0+a_1x+a_2x^2+\cdots+a_{n-1}x^{n-1}$来说，可以将其系数储存成一个向量$[a_0,a_1,a_2,\cdots,a_{n-1}]$，但如此做的话多项式相乘就是常规方法。
一旦多项式的项数很大的话，计算就会很麻烦。因此，应当寻找一种更为简便的方法。
其实，对于这样一个多项式，完全可以将其视为一个函数，$f(x)=a_0+a_1x+a_2x^2+\cdots+a_{n-1}x^{n-1}$，因此，除了系数表达法之外，还可以采取点值表示法。
如果已知一个函数为$f(x)=a_0+a_1x+a_2x^2+\cdots+a_{n-1}x^{n-1}$，那么，就可以找到该函数上的$n$个点来表示该函数，且可以唯一确定该函数，且这种表示方法是可以相互转换的，而且转换后的形式是唯一的。可以用矩阵表示其关系：
$$
\begin{pmatrix}
    &f(x_0)\\
    &f(x_1)\\
    &\vdots\\
    &f(x_{n-1})\\
\end{pmatrix}=
\begin{pmatrix}
    &1&x_0&x_0^2&\cdots&x_0^{n-1}\\
    &1&x_1&x_1^2&\cdots&x_1^{n-1}\\
    &\vdots&\vdots&\vdots&&\vdots\\
    &1&x_{n-1}&x_{n-1}^2&\cdots&x_{n-1}^{n-1}\\
\end{pmatrix}
\begin{pmatrix}
    &a_0\\
    &a_1\\
    &\vdots\\
    &a_{n-1}\\
\end{pmatrix}
$$
其中$(x_0,f(x_0),x_1,f(x_1),\cdots,x_{n-1},f(x_{n-1}))$是函数上的n个点。$[a_0,a_1,a_2,\cdots,a_{n-1}]$是其系数。

这样多项式相乘可以换成点值的相乘，再将点值转换成系数即可。流程图如下：

```mermaid
flowchart LR;
id1[取值]-->id2[相乘]-->id3[转换]
```

# 取值
如果只是简单随意取值，那么这种方式的计算方法也就和一般计算方法没有差距，可以在此做出优化。
注意到函数都是由积分量和偶分量组成的，由此可以将函数成如下形式：
$$
f(x)=f_e(x^2)+xf_o(x^2)
$$
举一个小例子，若$f(x)=a_0+a_1x+a_2x^2+a_3x^3$，那么有：
$$
\begin{aligned}
    f(x)&=a_0+a_2x^2+a_1x+a_3x^3\\
    令f_e(x^2)&=a_0+a_2x^2\\
    f_o(x^2)&=a_1+a_3x^2\\
    则f(x)&=f_e(x^2)+xf_o(x^2)
\end{aligned}
$$
由此，取的点是奇偶对的话，就可以由n个点变成$n/2$个点（当n为奇数时向上取整），这样计算量就相当于少了一半。但是，对于$f_e(x^2)和f_o(x^2)$来说，自变量是平方，不存在实数奇偶对，因此，就需要取值复数奇偶对。
那么取值就可以采用$z^n=1$的方式来取值，有过复数知识就知道复数的n次根式即使模值取n次根式，相角除n，那么$z^n=1$的解即为$(1,\omega,\omega^2,\cdots,\omega^{n-1})$那么就有：
$$
\begin{aligned}
    f(\omega^i)&=f_e(\omega^{2i})+\omega^i f_o(\omega^{2i})\\
    f(-\omega^i)&=f_e(\omega^{2i})-\omega^i f_o(\omega^{2i})\\
    \because -\omega^i&=\omega^{i+n/2}\\
    \therefore f(\omega^{i+n/2})&=f_e(\omega^{2i})-\omega^i f_o(\omega^{2i})
\end{aligned}
$$
那么这样，取值点就少了一半。

# 转换
上文已经完成了取值的优化，现在只需要ifft将点转化成系数即可，那么怎么进行ifft就是一个问题，已知系数和点之间的转化是唯一的，可用如下矩阵表示：
$$
\begin{pmatrix}
    &f(1)\\
    &f(\omega)\\
    &\vdots\\
    &f(\omega^{n-1})\\
\end{pmatrix}=
\begin{pmatrix}
    &1&1&1&\cdots&1\\
    &1&\omega&\omega^2&\cdots&\omega^{n-1}\\
    &\vdots&\vdots&\vdots&&\vdots\\
    &1&\omega^{n-1}&\omega^{2(n-1)}&\cdots&\omega^{(n-1)(n-1)}\\
\end{pmatrix}
\begin{pmatrix}
    &a_0\\
    &a_1\\
    &\vdots\\
    &a_{n-1}\\
\end{pmatrix}\\
$$
那么求解系数只需要求解中间矩阵的逆即可：
$$
\begin{pmatrix}
    &a_0\\
    &a_1\\
    &\vdots\\
    &a_{n-1}\\
\end{pmatrix}=
\begin{pmatrix}
    &1&1&1&\cdots&1\\
    &1&\omega&\omega^2&\cdots&\omega^{n-1}\\
    &\vdots&\vdots&\vdots&&\vdots\\
    &1&\omega^{n-1}&\omega^{2(n-1)}&\cdots&\omega^{(n-1)(n-1)}\\
\end{pmatrix}^{-1}
\begin{pmatrix}
    &f(1)\\
    &f(\omega)\\
    &\vdots\\
    &f(\omega^{n-1})\\
\end{pmatrix}\\
\because 是范德蒙矩阵所以得：
\\
\begin{pmatrix}
    &a_0\\
    &a_1\\
    &\vdots\\
    &a_{n-1}\\
\end{pmatrix}={1\over n}
\begin{pmatrix}
    &1&1&1&\cdots&1\\
    &1&\omega^{-1}&\omega^{-2}&\cdots&\omega^{-(n-1)}\\
    &\vdots&\vdots&\vdots&&\vdots\\
    &1&\omega^{-(n-1)}&\omega^{-2(n-1)}&\cdots&\omega^{-(n-1)(n-1)}\\
\end{pmatrix}
\begin{pmatrix}
    &f(1)\\
    &f(\omega)\\
    &\vdots\\
    &f(\omega^{n-1})\\
\end{pmatrix}
$$
由上式可以看出ifft其实是fft，只不过将值$\omega$取了倒数。由此就完成了转换。

# 代码实现
FFT就是其中取值的过程
```python
def FFT(p):
    n = len(p)
    if n==1:
        return p
    w = (-1)**(1/(n/2))
    fe,fo=p[::2],p[1::2]
    ye,yo=FFT(fe),FFT(fo)
    y = [0]*n
    for j in range(n/2):
        y[j]=ye[j]+w**j*yo[j]
        y[j+n/2]=ye[j]-w**j*yo[j]
    return y
```

iFFT就是其中转换的过程
```python
def iFFT(p):
    def FFT(p):
        n = len(p)
        if n==1:
            return p
        w = (-1)**(1/(n/2))
        fe,fo=p[::2],p[1::2]
        ye,yo=FFT(fe),FFT(fo)
        y = [0]*n
        for j in range(n/2):
            y[j]=ye[j]+w**j*yo[j]
            y[j+n/2]=ye[j]-w**j*yo[j]
        return y
    p = FFT(p)
    for i in len(p):
        p[i] = p[i]/len(p)
    return p
```



