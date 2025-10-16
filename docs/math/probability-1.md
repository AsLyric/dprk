---
title: 概率与期望好题
published: 2024-10-16
updated: 2025-09-20
tags: [数学, 概率与期望, 题集]
category: 信息学
draft: false 
---

#### [ZOJ3329]One Person Game

期望倒着推。

设 $f_i$ 表示期望还需多少次结束。

我们可以得到式子 

$$
f_i=(\sum f_{x+i}p_i)+f_0p_0+1
\tag{$1$}
$$

发现每一项都与 f0 有关。

我们直接换元得 

$$
f_i=a_i f_0+b_i\tag{$2$}
$$

将 $(2)$ 代入 $(1)$ 得
$$
\begin{gather}
f_i=(\sum(a_{x+i}f_0+b_{x+i})p_i)+f_0p_0+1\tag{$3$}\\
=f_0(p_0+\sum a_{x+i}p_i)+(\sum b_{x+i}p_i)+1\tag{$4$}
\end{gather}
$$

其中
$$
\begin{gather}
a_x=p_0+\sum a_{x+i}p_i\tag{$5$}\\
b_x=(\sum b_{x+i}p_i)+1\tag{$6$}
\end{gather}
$$

到这步就可以递推 $a,b$ 了，我们解方程 $f_0=a_0f_0+b_0$ 即可。

## 期望方程

看似有无限种可能，但是可以用方程求出。

例：抽卡的概率是 $p \in (0,1]$ ，求期望次数 $x$。

$x=1+(1-p)x \\ \Longrightarrow x=\frac{1}{p}$