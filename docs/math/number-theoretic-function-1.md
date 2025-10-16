---
title: 数论函数好题（一）
published: 2025-09-29
updated: 2025-09-29
tags: [数学, 数论]
category: 信息学
draft: false 
---

#### QOJ12403 Gcd Product

https://blog.nowcoder.net/n/1eb39f84672a494fb5c1c0ff75eade3b

$$
C(n)=\sum_{x|n}A(x)\sum_{y|n}B(y)\sum_{i=1}^n\sum_{d_1|\frac{n}{x}}\mu(d_1)[d_1|\frac{i}{x}]\sum_{d_2|\frac{n}{y}}\mu(d_2)[d_2|\frac{n-i+1}{y}]
$$

令 $T_1=xd_1,T_2=yd_2$，则有：

$$
C(n)=\sum_{T_1|n}\sum_{x|T_1}A(x)\sum_{T_2|n}\sum_{y|T_2}B(y)\sum_{i=1}^n\mu(\frac{T_1}{x})[T_1|i]\mu(\frac{T_2}{y})[T_2|n-i+1]\\
=\sum_{T_1|n}(A\star\mu)(T_1)\sum_{T_2|n}(B\star\mu)(T_2)\sum_{i=1}^n[T_1|i][T_2|n-i+1]
$$
我们预处理出 $A\star\mu$ 和 $B\star\mu$。

其中：

$$
\sum_{i=1}^n[T_1|i][T_2|n-i+1]\\
=\frac{n}{T_1T_2}[T_1\perp T_2]\\
$$

感觉有点难证。

得到：

$$
C(n)=\sum_{V|n}\frac{n}{V}\sum_{T_1|V}(A\star\mu)(T_1)(B\star\mu)(\frac{V}{T_1})[T_1\perp \frac{V}{T_1}]
$$

狄利克雷卷积可以 $O(n\log n)$ 算出前 $n$ 项（调和级数）。