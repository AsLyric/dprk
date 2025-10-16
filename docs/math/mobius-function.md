---
title: 莫比乌斯函数与莫比乌斯反演
published: 2025-01-22
updated: 2025-09-29
tags: [数学, 数论, 反演]
category: 信息学
draft: false 
---

# 前置知识

数论函数是定义域为 $\mathbb{N}_+$（正整数集合）的函数。常见的数论函数包括：

- **常数函数** $I(n) = 1$；
- **单位函数** $\epsilon(n) = [n = 1]$，即仅在 $n = 1$ 时取值 1，其余取 0；
- **恒等函数** $\text{id}(n) = n$；
- **约数个数函数** $\sigma(n)$，表示 $n$ 的约数个数……

[关于狄利克雷卷积](/math/math/dirichlet-convolution/)。

# 莫比乌斯函数

莫比乌斯函数 $\mu(n)$ 是一个积性函数，是构造出的满足 $I\star \mu=\epsilon$ 的函数，也即 $\sum_{d|n}\mu(d)=[n=1]$。

定义如下：

$$
\mu(n) =
\begin{cases}
1, & n = 1 \\
(-1)^k, & n = p_1 p_2 \cdots p_k, \text{ 其中 } p_1, p_2, \dots, p_k \text{ 是不同的素数} \\
0, & n \text{ 不是平方自由的数}
\end{cases}
$$

其中，“平方自由”指的是 $n$ 的素因数中没有任何因子重复出现（即没有因子为 $p^2$ 的形式）。

## 性质

1. $\sum_{d|n}\mu(d)=[n=1]$。
2. $\sum_{d|n}\frac{\mu(d)}{d}=\frac{\phi(n)}{n}$。

# 莫比乌斯反演

在充分理解莫比乌斯函数 $\mu(n)$ 后，我们可以使用莫比乌斯反演公式：对于两个数论函数 $f$ 和 $g$，如果满足：

$$ f = g \star I $$

则有：

$$ g = f \star \mu $$

很多时候 $g$ 并不好求，但 $f=g\star I$ 可能很好求，就可以使用莫比乌斯反演解决。

# 例题

#### P2568 GCD
#### P4450 双亲数
#### P3455 [POI 2007] ZAP-Queries
#### P2522 [HAOI2011] Problem b

# 鸣谢

1. [Post] [Note -「Mobius 反演」光速入门 - Rainybunny](https://www.cnblogs.com/rainybunny/p/14359098.html)