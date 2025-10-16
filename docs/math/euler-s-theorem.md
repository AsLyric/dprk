---
title: 欧拉定理与费马小定理
published: 2025-01-22
updated: 2025-09-29
tags: [数学, 数论]
category: 信息学
draft: false 
---

# 前置

[关于欧拉函数](/math/math/euler-s-totient-function/)。

# 欧拉定理

欧拉定理的内容是：对于任意的整数 $a, n \in \mathbb{N}_+$，如果 $\gcd(a, n) = 1$，则有：

$$ a^{\varphi(n)} \equiv 1 \pmod{n} $$

这意味着对于任意的 $a$ 和 $n$，当 $a$ 与 $n$ 互质时，$a^{\varphi(n)}$ 在模 $n$ 下总是等于 1。

# 欧拉定理的推论

欧拉定理的一个重要推论是：对于任意的 $a, b, n \in \mathbb{N}_+$，且 $\gcd(a, n) = 1$，有：

$$ a^b \equiv a^{b \bmod \varphi(n)} \pmod{n} $$

这个推论可以用于简化模幂运算，尤其是在模数较大的情况下。

## P1397 [NOI2013] 矩阵游戏

[链接](./matrix-multiplication/#p1397-noi2013-矩阵游戏)

# 扩展欧拉定理

当 $b \geq \varphi(n)$ 时，欧拉定理可进一步推广为：

$$ a^b \equiv a^{(b \bmod \varphi(n)) + \varphi(n)} \pmod{n} $$

该扩展的证明较为复杂，但它在一些实际应用中（如大整数运算）非常有用。

## CF906D Power Tower（欧拉降幂，重要⭐）

在处理区间 $[l, r]$，模 $m$ 的问题时，递归计算区间 $[l+1, r]$ 模 $\varphi(m)$。经过 $O(\log m)$ 次递归后，$m$ 会变为 1 或递归结束，从而保证了总的时间复杂度为 $O(q \log m)$。

根据扩展欧拉定理，如果结果 $\geq m$，模 $m$ 后再加上 $\varphi(m)$。

# 费马小定理

费马小定理是欧拉定理的一个特例。其内容是：对于质数 $p$ 和任意整数 $a$，有：

$$ a^p \equiv a \pmod{p} $$

此外，费马小定理还可以用于求模数为质数的逆元：对于质数 $p$ 和整数 $a$，有：

$$ a^{p-2} \equiv a^{-1} \pmod{p} $$

这为模运算提供了高效的算法。