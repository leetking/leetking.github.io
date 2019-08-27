---
title: 从傅里叶级数到傅里叶变换
create_date: 2019-08-26
modify_date: 2019-08-26
catergories: [mathematics, fourier]
---

> 越是深刻的道理越是简单，正如单字「无」

上次在文章「从正交基角度看待傅里叶级数」中讲到如何从正交基的角度理解并推导傅里叶级数，现在延续上次讲的思想，并且通过引入**欧拉函数**得到指数形式的傅里叶级数，然后对积分区间求极限得到**连续傅里叶变换**。


## 傅里叶级数回顾

先回顾下傅里叶级数的形式，定义在 $[t, t+T]$ 上的实数函数 $f(x)$，如果其满足**迪利克雷条件**那么它可以写成傅里叶级数的形式。具体如下：

$$
\begin{align}
f(x) &\sim \frac{a_0}{2} + \sum_{n=1}^{\infty}[a_n \cos(2\pi \frac{n}{T} x) + b_n \sin(2\pi \frac{n}{T} x)] \\
a_n &= \frac{2}{T}\int_t^{t+T} f(x) \cos(2\pi \frac{n}{T} x)\, \mathrm{d} x ,~ \forall n \ge 0 \\
b_n &= \frac{2}{T}\int_t^{t+T} f(x) \sin(2\pi \frac{n}{T} x)\, \mathrm{d} x ,~ \forall n \ge 1 \\
\end{align}
\tag{formular-1}
$$

前面说到傅里叶级数展开的过程也可以看成一个把函数 $f(x)$ 变换到我们找到的函数基 $\mathbb{B}_1$ 里面的一个点 $F$ 的变化 $\mathcal{F}$，同时函数 $f(x)$ 的所有信息能够通过点 $F$ 重构，这样的信息和其组织形式无关。

由于傅里叶级数的上述表示略显复杂，同时那个 $\frac{a_0}{2}$ 犹如一颗石头卡在心中。不妨采用如下的形式来表示傅里叶级数，而不是上述的「经典款」。对比 $a_n$、$b_n$ 和 $a_0$ 可以发现前二者都是后者的某种二倍的形式。不妨记

$$
\begin{align}
f(x) &\sim a_0 + \sum_{n=1}^{\infty}[2 a_n \cos(2\pi \frac{n}{T} x) + 2 b_n \sin(2\pi \frac{n}{T} x)] \\
    &= \sum_{n=-1}^{-\infty}[a_n \cos(2\pi \frac{n}{T} x) - b_n \sin(2\pi \frac{n}{T} x)]
       + a_0
       + \sum_{n=1}^{\infty}[a_n \cos(2\pi \frac{n}{T} x) + b_n \sin(2\pi \frac{n}{T} x)] \\
    &= \sum_{n=-\infty}^{+\infty}[a_n \cos(2\pi \frac{n}{T} x) + b_n \sin(2\pi \frac{n}{T} x)]
       \tag{formular-2} \\
a_n &= \frac{1}{T}\int_t^{t+T} f(x) \cos(2\pi \frac{n}{T} x)\, \mathrm{d} x ,~ n \in \mathbb{Z} \\
b_n &= \frac{1}{T}\int_t^{t+T} f(x) \sin(2\pi \frac{n}{T} x)\, \mathrm{d} x ,~ n \in \mathbb{Z} \\
\end{align}
$$

这样得到的系数表达式更加一致，形式更加简洁。此时有 $b_0 = 0$，在这种形式下点 $F$ 所对应的「坐标」为，$F = (\dots, a_{-n}, b_{-n}, \dots, a_0, b_0, \dots, a_n, b_n, \dots)$，且有 $a_n = a_{-n}$ 和 $b_n = -b_{-n}$。


## 傅里叶级数的指数形式

上述修改过的傅里叶级数展开式已经离指数形式表达不远了，只需要带入欧拉函数便可以写出。这里不打算走这条路，而是采取从变换后的结果导出表达式的方法。


### 改写点 $F$ 为函数形式

在前面把函数展开后的系数看成正交基空间的一个点的基础上更进一步，我们采取另外一种形式来组织点 $F$ 的坐标。把 $a_n$ 和 $b_n$ 看成一数对或者说平面上的一点，那么可以发现 $F$ 的坐标分量可以看成关于 $n$ 的函数，并且是一个从整数域到平面的函数，即 $F: \mathbb{Z} \to \mathbb{R}^2$。

$$
F(n) = (a_n, b_n), ~ n \in \mathbb{Z}
$$

由于复数和二维坐标的天然联系，我们不妨认为函数 $F$ 是一个从实数域到复平面的函数 $F: \mathbb{R} \to \mathbb{C}$。这里采取对应关系是 $(a_n, b_n) \to a_n - i b_n$，而不是 $(a_n, b_n) \to a_n+i b_n$，二者区别很美妙，后面自会明白。

$$
\begin{align}
F(n) &= a_n - i b_n \\
&= \frac{1}{T}\int_t^{t+T} f(x) \cos(2\pi \frac{n}{T} x)\, \mathrm{d} x - i \frac{1}{T}\int_t^{t+T} f(x) \sin(2\pi \frac{n}{T} x)\, \mathrm{d} x \\
&= \frac{1}{T}\int_t^{t+T} f(x) [\cos(2\pi \frac{n}{T} x) - i \sin(2\pi \frac{n}{T} x)]\, \mathrm{d} x \\
&= \frac{1}{T}\int_t^{t+T} f(x) e^{-i 2\pi \frac{n}{T} x}\, \mathrm{d} x, ~ n \in \mathbb{Z} \\
\end{align}
$$

这样傅里叶级数展开 $\mathcal{F}$ 可以理解为把函数 $f(x)$ 变换到另外一个复数函数 $F(n)$ 的过程。

为了方便后面叙述，引入函数 $\hat f(w)$，这样同时可以简化 $F(n)$ 的表达

$$
\hat f(w) = \int_t^{t+T} f(x) e^{-i 2\pi w x}\, \mathrm{d} x
\tag{formular-3}
$$

那么此时的 $F(n)$ 为

$$
F(n) = \frac{1}{T} \hat f(\frac{n}{T}), ~ n \in \mathbb{Z} \\
$$


### 求出变换 $\mathcal{F}$ （傅里叶级数）的指数形式

由于 $a_n$ 是 $F(n)$ 的实部，有 $a_n = \frac{F(n) + F(-n)}{2}$，$b_n$ 是 $F(n)$ 虚部的相反数 $b_n = \frac{F(-n)-F(n)}{2i}$，带入 (formular-2) 得到

$$
\begin{align}
f(x) &\sim \sum_{n=-\infty}^{+\infty}[a_n \cos(2\pi \frac{n}{T} x) + b_n \sin(2\pi \frac{n}{T} x)] \\
    &= \sum_{n=-\infty}^{+\infty}[\frac{F(n)+F(-n)}{2} \cos(2\pi \frac{n}{T} x) + \frac{F(-n)-F(n)}{2i} \sin(2\pi \frac{n}{T} x)] \\
    &= \frac{1}{2T} \sum_{n=-\infty}^{+\infty} \left\{\hat f(\frac{n}{T})[\cos(2\pi \frac{n}{T} x) + i \sin(2\pi \frac{n}{T})]
       + \hat f(-\frac{n}{T})[\cos(2\pi \frac{n}{T} x) - i \sin(2\pi \frac{n}{T})]\right\} \\
    &= \frac{1}{2T} \sum_{n=-\infty}^{+\infty} [\hat f(\frac{n}{T}) e^{i 2\pi \frac{n}{T} x} + \hat f(-\frac{n}{T}) e^{-i 2\pi \frac{n}{T} x}] \\
    &= \frac{1}{T} \sum_{n=-\infty}^{+\infty} \hat f(\frac{n}{T}) e^{i 2\pi \frac{n}{T} x} 
       \tag{formular-4} \\
\end{align}
$$

公式 (formular-4) 便是傅里叶级数的指数形式。$e^{i 2\pi \frac{n}{T} x}$ 便是指数形式下的基，并且这个基也是正交的，验证可知

$$
<e^{i 2\pi \frac{j}{T} x}, e^{i 2\pi \frac{k}{T} x}> = \int_t^{t+T}\,e^{i 2\pi \frac{j+k}{T} x}\, \mathrm{d}x = 0
$$

$F(n)$ 便是基下的系数。


## 傅里叶变换

我们知道傅里叶级数虽然是在区间 $[t, t+T]$ 上逼近函数 $f(x)$，但是级数本身收敛到的函数 $\tilde f(x)$ 放在 $\mathbb{R}$ 来看是一个周期函数，一个周期便是 $f(x)$。连续傅里叶变换是这个思想的延伸，我们希望傅里叶级数不只是在 $[t, t+T]$ 这个区间上逼近，更在整个实数域逼近，那么这个时候对于函数 $f(x)$ 可以简单的补充定义的得到 $f_1(x)$

$$
f_1(x) = \begin{cases}
f(x), ~ &x \in [t, t+T] \\
  0, ~ &x \notin [t, t+T] \\
\end{cases}
$$

同时尝试在实数域 $\mathbb{R}$ 上展开傅里叶级数。下面我们认为在 $[t, t+T]$ 上定义的函数 $f(x)$ 都已经被补充定义为 $f_1(x)$，并且下面说的 $f(x)$ 就是指代的 $f_1(x)$。

先任意找一个区间 $[-\frac{T_1}{2}, \frac{T_1}{2}] \supset [t, t+T]$，让 $f(x)$ 在此区间展开，并让 $T_1 \to \infty$，可以得到 $f(x)$ 在整个实数域的傅里叶级数展开（其实也就是连续傅里叶变换了）。那么对 $f(x)$ 使用 (formular-4) 公式

$$
f(x) = \lim_{T_1 \to \infty} \sum_{n=-\infty}^{+\infty} \hat f(\frac{n}{T_1}) e^{i2\pi\frac{n}{T}x} \frac{1}{T_1}
\tag{2-1}
$$

对比一下定义积分的定义

$$
\int_a^b\, f(x)\, \mathrm{d}x = \lim_{\Delta x \to 0} \sum_{i=0}^n f(\xi_i)\, \Delta x
$$

可以发现二者有一定的类似 $\frac{1}{T_1}$ 对应 $\Delta x$，$\frac{n}{T}$ 对应 $\xi_i$，从这个角度能不那么严谨但是利于理解地得到极限 (2-1) 的结果

$$
f(x) = \int_{-\infty}^{\infty} \hat f(w)e^{i2\pi w x}\, \mathrm{d} w
\tag{fourier-fransfrom-inv}
$$

此时的 $\hat f(w)$ 变成了

$$
\hat f(w) = \int_{-\infty}^{\infty} f(x) e^{-i 2\pi w x}\, \mathrm{d} x
\tag{fourier-transform}
$$

公式 (fourier-transform) 便是**连续傅立叶变换**，公式 (fourier-transform-inv) 便是**连续傅里叶逆变换**，二者形式都极其简洁，并且一致。两个变换的差别在于正负号而已，前面提到的认为 $(a_n, b_n) \to a_n - i b_n$ 这样得到的便是傅里叶变换，若让 $(a_n, b_n) \to a_n + i b_n$ 得到的便是傅里叶逆变换。

$\hat f(w)$ 是一个从实数到复数的函数，连续傅里叶变换是一个算子 $\mathcal{F}$：它接收一个函数求出其在正交基 $e^{2\pi w x}$ 下的「系数」的表示，并把这样的「系数」写成了关于**频率** $w$ 的函数。$\hat f(w)$ 也就是所谓的函数 $f(x)$ 的频域表达。连续傅里叶逆变换也是一个算子 $\mathcal{F}^{-1}$，而且是傅里叶变换的逆算子：它接收一个函数求出其在正交基 $e^{-2\pi w x}$ 下的「系数」表示，可以把它解读成从频域重组得到时域的表达。

$$
\begin{align}
\hat f(w) &= \mathcal{F}(f) \\
f(x) &= \mathcal{F}^{-1}(\hat f) = \mathcal{F}^{-1}(\mathcal{F}(f)) \\
\end{align}
$$


## 推荐阅读

1. 关于傅里叶变换的形象理解以及其与不确定性原理的关系，推荐 [3Blue1Brown](https://space.bilibili.com/88461692) 的视频。


## 补充

* 欧拉函数

欧拉等式或者叫欧拉函数 $e^{ix} = \cos x + i \sin x$，或者写成这里 $e^{i2\pi w x} = \cos(2\pi w x) + i \sin(2\pi w x)$ 的形式，从等式右边可见 $w$ 反映了周期函数 $e^{i2\pi w x}$ 的频率。例如：$w = 1$，$\cos(2\pi x)$ 和  $\sin(2\pi x)$ 的周期是 $1$，频率也是 $1$，那么 $e^{i 2\pi x}$ 的频率也是 $1$。
