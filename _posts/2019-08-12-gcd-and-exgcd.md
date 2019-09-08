---
title: 最大公约数和辗转相除法
create_date: 2019-08-12
modify_date: 2019-09-06
categories: [gcd, algorithm]
---

本文是我对最大公约数相关知识点的复习和总结。另外本文一些符号的含义、一些概念的定义也许和通常有些出入，以及使用了一些其他结论，请见文末「背景知识」。

**开门见山**。


## 最大公约数（Greatest Common Divisor）

**最大公约数**的概念是指两个整数共同的最大的约数。比如： $1, 2, 3, 6$ 都是 $6$ 的约数，$1, 3, 9$ 都是是 $9$ 约数。那么 $6$ 和 $9$ 的最大公约数便是 $3$， 记作 $\gcd(6, 9) = 3$。 一般地对于任意两个整数 $a, b$，其最大公约数记为 $\gcd(a, b)$。

明确了最大公约数的概念之后，接下来应该考虑如何求任意两个整数的最大公约数以及最大公约数有哪些性质。


## 辗转相除法

求最大公约数的算法有类似上面的把两个整数的所有约数都找出来，找出其中最大的相同的约数；中国古代也有求最大公约数的算法，「九章算术」中提到了「更相减损术」，本质上和接下来讨论的辗转相除法一样的，只是把除法换成了多次减法；还有就是这里主要讨论的辗转相除法以及从最大公约数性质得到的另外一个算法。

**辗转相除法**（也叫欧几里得算法）是最常用求解两个整数的最大公约数的算法。辗转相除法算法步骤如下：
```text
输入：两个整数 a, b
输出：a, b

step-1. 如果 b 等于 0，那么 a 和 b 的最大公约数便是 a；否则执行 step-2
step-2. 令 a' = b, b' = a % b，执行 step-3
step-3. 令 a = a', b = b'，执行 step-1
```

举一个例子🌰：求 $6$ 和 $9$ 的最大公约数。
```text
输入：a = 6，b = 9

1. 由于 9 不等于 0，那么执行 step-2
2. a' = 9, b' = 6 % 9 = 6 （因为 6 = 0 × 9 + 6）
3. a = 9, b = 6
4. 由于 6 不等于 0，那么执行 step-2
5. a' = 6, b' = 9 % 6 = 3 （因为 9 = 1 × 6 + 3）
6. a = 6, b = 3
7. 由于 3 不等于 0，那么执行 step-2
8. a' = 3, b' = 6 % 3 = 0 （因为 6 = 2 × 3 + 0）
9. a = 3, b = 0
10. 由于 b 现在是 0，所以程序结束并输出 3
```

根据上面的分析可以写出相应的 C 程序或其他语言的代码。
```c
int gcd(int a, int b)
{
    while (b != 0) {
        int aa = b, bb = a%b;
        a = aa;
        b = bb;
    }

    return a;
}
```


## 正确性和时间复杂度

### 正确性

由于任意两个整数一定存在最大公约数（无论如何 $1$ 都是任意整数的约数），不妨记 $g = \gcd(a, b)$，那么对于 $a$ 和 $b$ 可以写成：

$$
\begin{align}
a &= g \times k_1 \\
b &= g \times k_2
\end{align}
$$

接下来证明，$a$ 除以 $b$ 的余数 $r$ 与 $b$ 的最大公约数也是 $g$，也就是 $\gcd(a, b) = \gcd(b, a\%b)$。由于 $r$ 是 $a$ 除以 $b$ 的余数

$$
\begin{align}
a &= k \times b + r \\
r &= a - k \times b \\
  &= g \times k_1 - k \times g \times k_2 \\
  &= g \times (k_1 - k \times k_2) \\
\end{align}
$$

这里得到 $g$ 是 $r$ 的约数。并且由于 $k_1 - k \times k_2$ 和 $k_2$ 之间不会有其他非 $1$ 的公约数，那么 $r$ 和 $b$ 的最大公约数也是 $g$。这是由于 $k_1$ 和 $k_2$ 之间不会有其非 $1$ 公约数，那么 $\frac{k_1 - k \times k_2}{k_2} = \frac{k_1}{k_2} - k$ 一定不是整数。

这就说明 $\gcd(a_0, b_0) = \gcd(a_1, b_1) = \cdots = \gcd(a_n, b_n)$ 其中 $a_i = b_{i-1}, b_i = a_{i-1}\%b_{i-1}, \forall i \ge 1, a_0 = a, b_0 = b$，当 $b_n = 0$ 时，$a$ 和 $b$ 的最大公约数 $g = \gcd(a_n, b_n) = \gcd(a_n, 0) = a_n$。

上述序列的收敛性：由于 $b_i = a_{i-1}\%b_{i-1}$ 那么由余数定义可知 $b_{i-1} > b_i$。那么序列 $b_0, b_1, \dots, b_{n-1}, b_n$ 一定是递减且至少减少 $1$，对于任意一项 $b_i$ 为 $a_{i-1}$ 的余数，那么 $b_i \ge 0$。 $b_i$ 单调减少并且有下界，那么 $b_i$ 一定收敛到 $0$，说明此算法是收敛的，也就是 $b_n = 0$ 一定能取到。


### 时间复杂度

由上面证明可以知道，此算法也就是通过求余运算把输入的**数对** $(a, b)$ 一步步变换到 $(g, 0)$，的过程，这里的 $g$ 也就是 $a$ 和 $b$ 的最大公约数。还是以 $6$ 和 $9$ 的最大公约数的计算过程为例，上述过程可以看成 $(6, 9) \to (9, 6) \to (6, 3) \to (3, 0)$。可以看到每次运算都会使得 $a$ 和 $b$ 的绝对值减少，并且如果 $a$ 和 $b$ 中存在负数的话，经过最多两次变换就能得到求两个正数的最大公约数。如果把此过程产生一系列数对看成一个数列，分析下这个数列和「斐波那契数列」的关系，也许能得到数列长度，也就反映了算法的时间复杂度。

具体来说，考虑此过程中两次连续的变换过程 $(a_{i-1}, b_{i-1}) \to (a_i, b_i) \to (a_{i+1}, b_{i+1})$，此时 $a_{i-1}, b_{i-1}, a_i, b_i, a_{i+1}, b_{i+1} \ge 0$ 并且 $ a_{i-1} > b_{i-1}, a_i > b_i, a_{i+1} > b_{i+1}$。

$$
\begin{array}{rll}
a_i     &= b_{i-1} \\
b_i     &= a_{i-1} \% b_{i-1} &= a_{i-1} - k_{i-1} \times b_{i-1}  \\
a_{i+1} &= b_i \\
b_{i+1} &= a_i \% b_i         &= a_i - k_i \times b_i \\
\end{array}
$$

那么对于 $b_i + b_{i+1}$ 有

$$
\begin{align}
b_i + b_{i+1} &= b_i + a_i - k_i \times b_i \\
              &= a_i + (1 - k_i) \times b_i \\
              &= b_{i-1} + (1 - k_i) \times b_i \\
              &\le b_{i-1} \\
\end{align}
$$

也就是 $b_{i-1} \ge b_i + b_{i+1}$, 再考虑 $b$ 和 $Fib(n)$ 的关系

$$
\begin{array}{llll}
b_n     &= 0               &\ge 0               &= Fib(0) \\
b_{n-1} &= a_n             &\ge 1               &= Fib(1) \\
b_{n-2} &\ge b_{n-1} + b_n &\ge Fib(1) + Fib(0) &= Fib(2) \\
\dots \\
b_1     &\ge b_2 + b_3     &\ge Fib(n-2) + Fib(n-3) &= Fib(n-1) \\
b_0     &\ge b_1 + b_2     &\ge Fib(n-1) + Fib(n-2) &= Fib(n) \\
\end{array}
$$

那么 $b = b_0 \ge Fib(n) \approx \frac{1}{5} \phi^n$，两边求对数 $n \le (\log \phi - \log \sqrt{5}) \log b$，也就是

$$
n = \Omicron (\log b)
$$

得到结论：辗转相除法的时间复杂度为 $\Omicron (\log b)$， 这里 $b$ 为待求的两个整数中绝对值小的那一个。


## 最大公约数的性质

最大公约数具有如下性质：

$$
\begin{align}
\gcd(a, 0) &= a \\
\gcd(a, b) &= \gcd(b, a) \\
\gcd(a, b) &= \gcd(\vert a \vert, \vert b \vert) \\
\gcd(a, b) &= \gcd(b, a-b)\quad \text{假定}\ a \ge b \tag{attr-1} \\
\gcd(a, b) &= \gcd(b, a\%b) \\
\gcd(a, b) &= k \times \gcd(a/k, b/k)\quad  \text{如果} \  k \mid a,\, k \mid b \tag{attr-2} \\
\gcd(a, b) &= \gcd(a, b/k)\quad \text{如果} \  k \nmid a, \, k \mid b \tag{attr-3} \\
\gcd(a, b) &= 1\quad \text{如果}\  a, b \ \text{是质数} \\
\gcd(a, bc) &= \gcd(a, b) \gcd(a, c) \\
\gcd(a, b, c) &= \gcd(a, \gcd(b, c)) = \gcd(\gcd(a, b), c) \\
\end{align}
$$

上述性质不难证明。此外如果 $\gcd(a, b) = 1$，称 $a, b$ 互质。


### 最大公约数的几何解释

最大公约数从几何角度来看的话，$n = \gcd(a, b) + 1 = g + 1$ 表明线段 `(0, 0) -- (a, b)` 经过了 $g + 1$ 个整数坐标点。因为 $a = g \times a'$，可以理解为 $g$ 个 $a$ 相加，反映到坐标轴上就是 $g$ 段等长线段 $a'$ 构成了线段 $a$。

例如： $\gcd(9, 6) = 3$，线段 `(0, 0) -- (9, 6)` 经过了 `(0, 0)`、`(3, 2)`、`(6, 4)` 和 `(9, 6)` 这四个整数坐标点。

![gcd(9, 6) = 3](/images/gcd-geometry-explaintion.svg)


### 贝祖等式

**贝祖等式**是指对于任意整数 $a, b$ 一定存在整数 $x, y$ 使得，$x \times a + y \times b = \gcd(a, b)$。

从上述小节「时间复杂度」中的数对变换来推导可以轻松得到此结论，并且由扩展欧几里得算法能解出 $x$ 和 $y$，能得到所谓的模 $m$ 运算下的**逆元**。

数对 $(a_{i-1}, b_{i-1})$ 变换到 $(a_i, b_i)$，也就是 $a_i = b_{i-1},\, b_i = a_{i-1} - k_{i-1} \times b_{i-1}$ 可以写成矩阵形式

$$
\begin{bmatrix}
0 & 1 \\
1 & -k_{i-1} \\
\end{bmatrix}
\begin{bmatrix}
a_{i-1} \\
b_{i-1} \\
\end{bmatrix}
=
\begin{bmatrix}
a_i \\
b_i \\
\end{bmatrix}
$$

这个过程从$(a_0, b_0)$ （也就是 $(a, b)$）开始不停变换下去直到 $b_n = 0$，得到

$$
\begin{bmatrix}
0 & 1 \\
1 & -k_{n-1} \\
\end{bmatrix}
\cdots
\begin{bmatrix}
0 & 1 \\
1 & -k_1 \\
\end{bmatrix}
\begin{bmatrix}
0 & 1 \\
1 & -k_0 \\
\end{bmatrix}
\begin{bmatrix}
a_0 \\
b_0 \\
\end{bmatrix}
=
\begin{bmatrix}
a_n \\
b_n \\
\end{bmatrix}
=
\begin{bmatrix}
a_n \\
0 \\
\end{bmatrix}
\tag{eq-1}
$$

其中 $k_i = \left \lfloor \frac{a_i}{b_i} \right \rfloor$。上述任意一个矩阵其行列式为 $-1 \neq 0$，那么这些矩阵相乘得到

$$
\begin{bmatrix}
x  & y\\
k  & m\\
\end{bmatrix}
\begin{bmatrix}
a_0 \\
b_0 \\
\end{bmatrix}
=
\begin{bmatrix}
a_n \\
0 \\
\end{bmatrix}
$$

这里 $x, y, k, m$ 都是可以确定的整数，并且 $x \times a_0 + y \times b_0 = a_n \Rightarrow x \times a + y \times b = \gcd(a, b)$ 这便是贝祖等式。

特别地，令 $b = m$ 且 $a, m$ 互质，便有 $x \times a + y \times m = \gcd(a, m) = 1$，再对两边都求 $m$ 的余数得到

$$
x \times a \equiv 1 \pmod{m}
$$

这表示 $x \times a$ 和 $1$ 在模 $m$ 下**同余**，其中 $x$ 为 $a$ 在模 $m$ 下的逆元。


## 扩展欧几里得算法

贝祖等式中的 $x, y$ 可以通过 (eq-1) 中计算矩阵连乘得到，计算过程既可以从右向左计算，这样好处不需要递归只需要迭代即可，但是要维护一个矩阵（四个数）；也可以从左向右计算，好处是只需要维护两个数 $x, y$，但是需要递归计算（好在这个递归不深于$\log b$）。这里采取第二种方法，对于 (eq-1) 把 $x, y$ 添加进去，`?` 表示不关心这个数是多少，也不需要计算。

$$
\begin{bmatrix}
x_0 & y_0 \\
? & ? \\
\end{bmatrix}
\begin{bmatrix}
0 & 1 \\
1 & -k_{n-1} \\
\end{bmatrix}
\cdots
\begin{bmatrix}
0 & 1 \\
1 & -k_1 \\
\end{bmatrix}
\begin{bmatrix}
0 & 1 \\
1 & -k_0 \\
\end{bmatrix}
=
\begin{bmatrix}
x_1 & y_1 \\
? & ? \\
\end{bmatrix}
\begin{bmatrix}
0 & 1 \\
1 & -k_{n-2} \\
\end{bmatrix}
\cdots
\begin{bmatrix}
0 & 1 \\
1 & -k_1 \\
\end{bmatrix}
\begin{bmatrix}
0 & 1 \\
1 & -k_0 \\
\end{bmatrix}
$$

考察 $x_{i-1}, y_{i-1}$ 与 $x_i, y_i$ 的关系，有

$$
\begin{align}
x_i &= y_{i-1} \\
y_i &= x_{i-1}-k_{n-i} \times y_{i-1} = x_{i-1}-\left \lfloor \frac{a_{n-i}}{b_{n-i}} \right \rfloor \times y_{i-1}\\
x_0 &= 1 \\
y_0 &= 0 \\
\end{align}
$$

可以发现此式子和求最大公约数的变换过程很类似 $a_i = b_{i-1},\, b_i = a_{i-1} - k_{i-1} \times b_{i-1}$。

按照此关系得到扩展欧几里得算法

```c
/**
 * 输入两个整数 a, b 和存储 x 和 y 的结果的指针
 * 函数返回值为 a 和 b 的最大公约数
 */
int exgcd(int a, int b, int *x, int *y)
{
    if (0 == b) {
        *x = 1;
        *y = 0;
        return a;
    }
    int x1, y1;
    int g = exgcd(b, a%b, &x1, &y1);
    *x = y1;
    *y = x1 -a/b * y1;          /* C 语言里运算符 / 对整数便是下取整 */
    return g;
}
```

### 模 $m$ 逆元

对扩展欧几里得算法做简单改进得到求逆元的算法

```c
int inv(int a, int m)
{
    int x, _, g;
    g = exgcd(a, m, &x, &_);
    if (1 != g)
        return -1;      /* a 和 m 并不互质 */

    /* 逆元一般定义为正数，且小于模 m */
    if (x < 0)
        return x+m;
    else
        return x;
}
```

## 另一种求最大公约数的算法

最后以一种不需要「除法」的算法来求最大公约数作为本文的结束。所谓的不需要除法是指里面仅有的除法便是除以 $2$，这个可以写成二进制表示下的右位移一位的形式。

在现在基于二进制的计算机里，$2 * a \Leftrightarrow a \ll 1$ 并且 $ a/2 \Leftrightarrow a \gg 1$，这里的 $\ll$ 和 $\gg$ 表示右位移和左位移。

```text
输入：输入两个正整数 a, b
输出：a, b 的最大公约数
辅助变量：g = 1

step-1. 如果 b 等于 0，最大公约数就是 g*a；否则执行 step-2
step-2. 如果 a 和 b 都是偶数，那么 g = g<<1， a = a>>1, b = b>>1；否则执行 step-3
step-3. 如果 a 是偶数，b 是奇数，那么 a = a>>1；否则执行 step-4
step-4. 如果 b 是偶数，a 是奇数，那么 b = b>>1；否则执行 step-5
step-5. 如果 a 和 b 都是奇数，那么 a' = b, b' = abs(a-b)，a = a', b = b'，执行 step-1
```

`step-2` 的正确性由 (attr-2) 保证， `step-3` 和 `step-4` 的正确性由 (attr-3) 保证，`step-5` 的正确性由 (attr-1) 保证。这个算法的时间复杂度还是 $\Omicron(\log b)$，这里不做证明（从存在除以 $2$ 入手可以证明）。

下面给出一个 C 语言实现
```c
#define abs(x)  ((x)<0? -(x): (x))

int gcd2(int a, int b)
{
    int g = 1;
    while (b != 0) {
        if (a&1 && b&1) {
            int t = a;
            a = b;
            b = abs(t-b);
        } else if (a&1) {
            b >>= 1;
        } else if (b&1) {
            a >>= 1;
        } else {
            g <<= 1;
            a >>= 1;
            b >>= 1;
        }
    }

    return g*a;
}
```


## 背景知识

* 约数、整除

对于任意整数 $n$（包括正整数和负整数），其约数是指存在**正**整数 $m$ 和整数 $k$ 使得 $n = k \times m$ 成立，那么称正整数 $m$ 为 $n$ 的**正约数**，简称**约数**。这里的概念限制了约数只能是正整数，其实如果允许约数是负数，那么 $k$ 也是约数，这两种定义没有多大差别。

例如：$-6$ 的约数和 $6$ 的约数一样都是 $1, 2, 3, 6$。

如果 $m$ 是 $n$ 的约数，那么也就是 $m$ 能整除 $n$，记作 $m \mid n$，如果 $m$ 不能整除 $n$，记作 $m \nmid n$。

* 余数

对于任意整数 $n$ 和非零整数 $m$，存在唯一整数 $k$ 和 $r$ （这里 $0 \le r < \vert m \vert$），使得 $n = k \times m + r$ 成立，那么 $r$ 便是 $n$ 除以 $m$ 的**余数**。记作 $ r = n \mod m$ 或者 $r = n\%k$。

例如：$6$ 除以 $4$ 的余数是 $2$，因为 $6 = 1 \times 4 + 2$。

* 下取整

对一个实数向下取整，是指这样的一个函数 $y = \lfloor x \rfloor,\  x \in \mathbb{R}$，把实数 $x$ 映射到小于自身的最大的整数 $y$。

比如：$\lfloor 0.5 \rfloor = 0$、$\lfloor -0.2 \rfloor = -1$。

* 斐波那契数列

**斐波那契数列**是指这样的数列：

$$1, 1, 2, 3, 5, 8, 13,, 21, \cdots$$

其后一项是前两项的和，并且第一项和第二项是 $1$。递归定义如下：

$$
Fib(n) = \begin{cases}
0 & n = 0 \\
1 & n = 1 \\
Fib(n-1) + Fib(n-2) & n \ge 2 \\
\end{cases}
$$

并且通项是

$$
\begin{align}
Fib(n) &= \frac{1}{\sqrt{5}} \left[\left(\frac{1+\sqrt{5}}{2}\right)^n - \left(\frac{1-\sqrt{5}}{2}\right)^n\right] \\
       &\approx \frac{1}{\sqrt{5}} \phi^n \\
\end{align}
$$

其中 $\phi = \frac{1+\sqrt{5}}{2}$。此通项公式可从变换角度并且归约变换得到，也可以从待定系数并解两个系数方程得到。

* 矩阵乘法

一个矩阵便是一个变换，矩阵也是信息的记录方式。

方程组

$$
\begin{cases}
a \times x + b \times y &= e \\
c \times x + d \times y &= f \\
\end{cases}
$$

可以写成矩阵向量相乘的形式

$$
\begin{bmatrix}
a & b \\
c & d \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
\end{bmatrix}
=
\begin{bmatrix}
e \\
f \\
\end{bmatrix}
$$


## 参考资料

1. 计算机程序设计艺术【卷2】
