---
title: 从正交基角度看待傅里叶级数
create_date: 2019-08-07
modify_date: 2019-08-16
categories: [mathematics, fourier]
---

看过一些关于傅里叶级数的推导的文章，这些文章都是从正／余弦函数叠加的角度来解释并推导傅里叶级数的，虽然我也是从这个角度来初步理解傅里叶级数的思想，但是其中有一个问题：为何用于叠加的函数为什么一定是这个样子 $\sin(nwx+\phi_n)$，而不是 $\sin(n w_n x+\phi_n)$ （每个正弦函数的频率都是 $w$ 的整数倍而不是任意频率）一致困扰着我。

直到看了线性代数尤其是线性空间那部分知识，感觉从线性表示的角度来看从傅里叶级数的表示，并从选取正交基的角度来理解的话，就能明白这样的选择能保证所选「基」的正交。

最终这篇文章按照自己的理解从基角度直接写出傅里叶级数的表示，并求解出系数，当然其中有很多不严谨的地方，比如级数何时收敛问题，函数能否有傅里叶级数展开问题，无穷个积分式子能否有「分配率」问题……但是本文只是自己理解傅里叶的一个过程的记录，严格的证明就不管了 XD

假定函数 $f(x)$ 在定义域上都能展开为傅里叶级数，并且级数也收敛到函数 $f(x)$。


## 预备知识

* 函数的内积和正交

函数 $f(x)$ 和函数 $g(x)$ 是 $[a, b] \in \mathbb{R} \to \mathbb{R}$ 的实数函数，并且在定义域上可积。那么类似向量的内积，定义两函数在区间 $[a, b]$ 的内积为：

$$ <f(x),\, g(x)>\, \overset{\mathrm{def}}{=} \int_a^b\, f(x)g(x)\, \mathrm{d}x $$

特别地，当 $<f(x),\, g(x)>\,= 0$时，称函数在区间 $[a, b]$ 正交。


* 三角函数族正交性

三角函数族为 $\mathbb{B} = \\{1, \sin(x), \cos(x), \dots, \sin(nx), \cos(nx), \dots\\},\ n \in \mathbb{Z}^+$，在区间 $[-\pi, \pi]$ 两两正交。可以验证（由积分区间对称性和被积函数奇偶性）：

$$
\begin{align}
\int_{-\pi}^{\pi}\, \sin(ix)\sin(jx)\, \mathrm{d}x &= 0,\ \forall i, j \in \mathbb{Z}\ i \neq j \\
\int_{-\pi}^{\pi}\, \cos(ix)\cos(jx)\, \mathrm{d}x &= 0,\ \forall i, j \in \mathbb{Z}\ i \neq j \\
\int_{-\pi}^{\pi}\, \sin(ix)\cos(jx)\, \mathrm{d}x &= 0,\ \forall i, j \in \mathbb{Z}
\end{align}
\tag{1-2}
$$

这样就在区间 $[-\pi, \pi]$ 找到一个正交基，虽然其维度是无穷大。


## 先讨论：函数 $f(x)$ 是定义在关于原点对称的区间 $[-\tfrac{T}{2}, \tfrac{T}{2}],\ T > 0$ 上的实数函数

### 1. 构造基

傅里叶级数从线性代数基的角度理解的话，虽然我们通常的基只是一些「数字」但是傅里叶级数「创造性」地把函数 $f(x)$ 用在其定义域上的一个正交基来表示，并且这里的「基向量」就是三角函数族。这样的基向量其实一系列的正弦和余弦函数（当然包括常数1，这就是函数 $f(x)$ 所谓的直流分量的的基向量），函数的傅里叶级数也就是表示成这样的基向量的「线性」组合。但是函数 $f(x)$ 的定义域是 $[-\tfrac{T}{2}, \tfrac{T}{2}]$ 上述三角函数族 $\mathbb{B}$ 在这个区间不一定两两正交，好在我们对上述三角函数族（也就是式子(1-2)）可以做定积分换元 $ x \to 2\pi\tfrac{1}{T}x $，能得到：

$$
\begin{align}
\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, \sin(2\pi\frac{i}{T} x)\sin(2\pi\frac{j}{T} x)\, \mathrm{d}x &= 0,\ \forall i, j \in \mathbb{Z}\ i \neq j \\
\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, \cos(2\pi\frac{i}{T} x)\cos(2\pi\frac{j}{T} x)\, \mathrm{d}x &= 0,\ \forall i, j \in \mathbb{Z}\ i \neq j \\
\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, \sin(2\pi\frac{i}{T} x)\cos(2\pi\frac{j}{T} x)\, \mathrm{d}x &= 0,\ \forall i, j \in \mathbb{Z}
\end{align}
\tag{2-1}
$$

也就是调整其频率 $n$ 为 $2\pi\tfrac{n}{T}$ 那么得到新的基

$$ \mathbb{B_1} = \{1, \sin(2\pi\frac{1}{T} x), \cos(2\pi\frac{1}{T} x), \dots, \sin(2\pi\frac{n}{T} x), \cos(2\pi\frac{n}{T} x), \dots \},\ n \in \mathbb{Z}^+ $$

在区间 $[-\tfrac{T}{2}, \tfrac{T}{2}]$ 便是正交基，这个基和普通的基有点不一样，他的维度是无穷维。


### 2. 用基 $\mathbb{B_1}$ 表示函数 $f(x)$

接下来在 $f(x)$ 的定义域区间用基表示出：


$$
f(x) = A_0 * 1+ \sum_{n=1}^{\infty}[A_n\cos(2\pi\frac{n}{T} x)+B_n\sin(2\pi\frac{n}{T} x)]
\tag{2-2}
$$

可以理解 $f(x)$ 是在基 $\mathbb{B_1}$ 决定的线性空间内的一个点，并且「坐标」就是 $(A_0, A_1, B_1, \dots, A_n, B_n, \dots)$。


### 3. 求解出「坐标」

有两种可能方法，第一种类似泰勒级数一样的方法，不断对 $f(x)$ 求导数，建立级数求导和 $f(x)$ 导数的关系。这种方法在这里有点复杂就不采用了。第二种方法需要一点点观察，考虑到式子(2-1)后两个等式中当 $j=1$ ，有

$$
\begin{align}
\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, \cos(2\pi\frac{i}{T} x)\, \mathrm{d}x &= 0 \\
\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, \sin(2\pi\frac{i}{T} x)\, \mathrm{d}x &= 0
\end{align}
$$

那么对式子(2-2)两边求在区间 $[-\tfrac{T}{2}, \tfrac{T}{2}]$ 上的积分有

$$
\begin{align}
\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, f(x)\, \mathrm{d}x
&= \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, A_0 \, \mathrm{d}x
+ \sum_{n=1}^{\infty}[A_n \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, \cos(2\pi\frac{n}{T} x)\, \mathrm{d}x
+B_n \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, \sin(2\pi\frac{n}{T} x)\, \mathrm{d}x] \\
&= T * A_0 \\
\end{align}
$$

得到

$$
A_0 = \frac{1}{T} \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, f(x)\, \mathrm{d}x
\tag{formula-1}
$$

类似的方法求 $A_i\, (i \geq 1)$，先对(2-2)两边同时乘以 $\cos(2\pi \tfrac{i}{T} x)$，然后再求在 $[-\tfrac{T}{2}, \tfrac{T}{2}]$ 上的积分

$$
\begin{align}
f(x)\cos(2\pi \frac{i}{T} x) &= A_0 * \cos(2\pi \frac{i}{T} x)+ \sum_{n=1}^{\infty}[A_n\cos(2\pi\frac{n}{T} x)\cos(2\pi \frac{i}{T} x)+B_n\sin(2\pi\frac{n}{T} x)\cos(2\pi \frac{i}{T} x)] \\
\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, f(x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x
&= \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, A_0 * \cos(2\pi \frac{i}{T} x)\, \mathrm{d}x
+\sum_{n=1}^{\infty}[A_n\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\,\cos(2\pi\frac{n}{T} x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x
+ B_n\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\,\sin(2\pi\frac{n}{T} x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x]
\end{align}
$$

仅有当 $n = i$ 时，$\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\,\cos(2\pi\frac{n}{T} x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x$ 不为 $0$，而式子右边其余积分无论 $n$ 为多少是 $0$，那么得到

$$
\begin{align}
\int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, f(x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x
&= A_i \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\,\cos(2\pi\frac{i}{T} x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x \\
&= A_i \frac{T}{2\pi} \int_{-\pi}^{\pi}\, \cos^2(ix)\, \mathrm{d}x \\
&= A_i \frac{T}{2\pi} \int_{-\pi}^{\pi}\, [\frac12+\frac12\cos(2ix)]\, \mathrm{d}x \\
&= A_i \frac{T}{2}
\end{align}
$$

求出 $A_i$ 为

$$
A_i = \frac{2}{T} \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, f(x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x
\tag{formula-2}
$$

同样的方法可以求出 $B_i$

$$
B_i = \frac{2}{T} \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, f(x)\sin(2\pi \frac{i}{T} x)\, \mathrm{d}x
\tag{formula-3}
$$

对比 (formula-1)、(formula-2) 可以发现 $A_0$ 和 $A_i$ 极其相似。那么我们让 $a_0 = 2A_0$，$a_i = A_i$，$b_i = B_i$，最终得到如下表达式，这就是傅里叶级数的系数表达式：

$$
\begin{align}
a_i &= \frac{2}{T} \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, f(x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x, \ \forall i \geq 0 \\
b_i &= \frac{2}{T} \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, f(x)\sin(2\pi \frac{i}{T} x)\, \mathrm{d}x, \ \forall i \geq 1
\end{align}
$$

同时改写 (2-2) 为

$$
f(x) = \frac{a_0}{2} + \sum_{n=1}^{\infty}[a_n\cos(2\pi\frac{n}{T} x)+b_n\sin(2\pi\frac{n}{T} x)]
\tag{2-3}
$$

以便用上述新的系数来表示。

## 再讨论： $f(x)$ 的定义域不是关于原点对称，而是任意区间 $[t, t+T]$

仔细观察上述式子 (2-3) 我们可以发现，右边在区间 $[-\tfrac{T}{2}, \tfrac{T}{2}]$ 是收敛于 $f(x)$，但是却不仅仅如此，式子右边如果放在整个实数域 $\mathbb{R}$ 上来看的话，而且是周期为 $T$ 的周期函数！那么我们如果平移函数 $f(x)$ 的到整个实数域 $\mathbb{R}$，得到新函数 $\tilde f(x)$

$$
\tilde f(x) = f(x-nT),\  -\frac{T}{2} \leq x-nT \leq \frac{T}{2},\, n \in \mathbb{Z}
$$

并且函数 $\tilde f(x)$ 也是周期为 $T$ 的周期函数，每一个周期就是 $f(x)$。

基于上面的想法，我们对定义域为任意区间 $[t, t+T]$ 的函数 $f(x)$，也做类似的平移得到定义于 $\mathbb{R}$ 上的以 $T$ 为周期的函数 $\tilde f(x)$，并截取 $\tilde f(x)$ 在关于原点对称的区间 $[-\tfrac{T}{2}, \tfrac{T}{2}]$ 上的函数为 $f^1(x)$。

$f(x)$、$\tilde f(x)$ 和 $f^1(x)$ 的关系如下图

![$f(x)$、$\tilde f(x)$ 和 $f^1(x)$ 的关系](/images/fourier-series-3-1.svg)

$f^1(x)$ 类似于把 $f(x)$ 「折断」然后平移拼接在区间 $[-\tfrac{T}{2}, \tfrac{T}{2}]$ 上。我们知道 $f^1(x)$ 有傅里叶级数（记为 $s(x)$吧），并且 $s(x)$ 也是周期为 $T$ 的周期函数，并且级数也收敛到 $\tilde f(x)$。那么表明 $s(x)$ 也是收敛到 $f(x)$ 的，换句话说 $f(x)$ 能在 $[t, t+T]$ 上展开为级数 $s(x)$。

还剩下一个小细节，那么就是 $[-\tfrac{T}{2}, \tfrac{T}{2}]$ 上展开得到的 $s(x)$ 的系数能用关于 $f(x)$ 的积分来表示吗？答案是可以。以系数 $a_i$ 为例

$$
\begin{align}
a_i &= \frac{2}{T} \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, f^1(x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x \\
    &= \frac{2}{T} \int_{-\tfrac{T}{2}}^{\tfrac{T}{2}}\, \tilde f(x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x \\
    &= \frac{2}{T} \int_t^{t+T}\tilde f(x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x \\
    &= \frac{2}{T} \int_t^{t+T}\, f(x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x
\end{align}
$$

第二步用到了周期函数积分的一个性质。

至此，$f(x)$ 在任意定义域 $[t, t+T]$ 上都可以展开为傅里叶级数，其完整表示为：

$$
\begin{align}
f(x) &= \frac{a_0}{2} + \sum_{n=1}^{\infty}[a_n\cos(2\pi\frac{n}{T} x)+b_n\sin(2\pi\frac{n}{T} x)] \\
a_i &= \frac{2}{T} \int_t^{t+T}\, f(x)\cos(2\pi \frac{i}{T} x)\, \mathrm{d}x, \ \forall i \geq 0 \\
b_i &= \frac{2}{T} \int_t^{t+T}\, f(x)\sin(2\pi \frac{i}{T} x)\, \mathrm{d}x, \ \forall i \geq 1 \\
\end{align}
$$

## 傅里叶级数的进一步理解

按照上面关于基的理解，傅里叶级数展开的过程是一个变换（记为 $\mathcal{F}$），这个变换把函数 $f(x)$ 从实数域转变到了我们找到的基 $\mathbb{B_1}$ 决定的线性空间内的一个点 $F = (A_0, A_1, B_1, \dots, A_n, B_n, \dots)$。用映射来表示的话： $\mathcal{F}: \mathbb{F} \to \mathbb{B_1}$，一个从函数空间 $\mathbb{F}$ 到 $\mathbb{B_1}$ 空间的映射，它的输入是一个函数，输出是一个「点」。

从这个意义上来说，点 $F$ 就是等价于函数 $f(x)$，因为点中包含了函数的所有信息，或者说我们通过点也就能「重现」出函数。既然点就能代表函数，我们可以是否可以找一种更加好的方式来组织点的这些坐标呢?

答案是肯定的，而且通过这种方式可以导出傅里叶变换。


## 参考资料

1. [ElpsyCongree 傅里叶级数的推导](https://zhuanlan.zhihu.com/p/41455378)
2. [3Blue1Brown 形象展示傅里叶变换](https://www.bilibili.com/video/av19141078)
