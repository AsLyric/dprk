---
title: 置换
published: 2025-09-20
updated: 2025-09-20
tags: [数学]
category: 信息学
draft: false
---

一个集合 $X$ 到自身的双射（即一一对应）$\sigma$ 称为 $X$ 的一个**置换（permutation）**。

> 置换表示的是元素间的对应关系，而不关心元素是什么和顺序。讨论大小为 $n$ 的集合上的置换，通常假定集合就是 $\{1,2,\dots,n\}$，并以数的顺序为自然顺序。

## 表示

### 双行记号表示

集合 $X=\{x_1,x_2,\dots,x_n\}$ 上的置换表示为：
$$
\sigma=\begin{pmatrix}
 x_1 & x_2 & \dots & x_n \\
 x_{p_1} & x_{p_2} & \dots & x_{p_n}
\end{pmatrix}
$$
首行元素是什么顺序都可以。

### 图论表示

将 $x_i\to x_{p_i}$ 建图，得到若干个环。

### 轮换表示

置换可被分解为若干个轮换。

## 复合

给定两个置换：
$$
\sigma=\begin{pmatrix}
 x_1 & x_2 & \dots & x_n \\
 x_{p_1} & x_{p_2} & \dots & x_{p_n}
\end{pmatrix},\pi=\begin{pmatrix}
 x_{p_1} & x_{p_2} & \dots & x_{p_n} \\
 x_{q_1} & x_{q_2} & \dots & x_{q_n}
\end{pmatrix}
$$
他们的乘积 $\pi\circ\sigma$ 为：
$$
\begin{pmatrix}
 x_1 & x_2 & \dots & x_n \\
 x_{q_1} & x_{q_2} & \dots & x_{q_n}
\end{pmatrix}
$$
$(\pi\circ\sigma)(x)=\pi(\sigma(x))$，置换的复合从右向左运算，先经过 $\sigma$ 的映射，再经过 $\pi$ 的映射。置换的复合并不满足交换律。

## 逆置换

给定置换：
$$
\sigma=\begin{pmatrix}
 x_1 & x_2 & \dots & x_n \\
 x_{p_1} & x_{p_2} & \dots & x_{p_n}
\end{pmatrix}
$$


其逆置换可以被表示为：
$$
\sigma^{-1}=\begin{pmatrix}
 x_{p_1} & x_{p_2} & \dots & x_{p_n}\\
 x_1 & x_2 & \dots & x_n
\end{pmatrix}
$$
对于轮换表示的形式，将每个轮换中的元素书写顺序反转。

对于图论表示的形式，将每条边反向。

$$
(p^{-1}\circ p)(x)=(p\circ p^{-1})(x)=x
$$

$$
(p\circ q)^{-1}=q^{-1}\circ p^{-1}
$$

## 轮换

**轮换（cycle）**本身是特殊的置换。从任何一点 $x$ 出发都能通过反复应用置换 $\sigma$ 得到轮换中零一点 $y$。长度为 $k$ 的轮换被称为**k-轮换（k-cycle）**。

1-轮换被称为不动点，2-轮换被称为对换。

## 例题

#### AGC031D A Sequence of Permutations

易得 $f(p,q)=q\circ p^{-1} $。

于是：
$$
a_1=p\\
a_2=q\\
a_3=q\circ p^{-1}\\
a_4=q\circ p^{-1}\circ q^{-1}\\
a_5=q\circ p^{-1}\circ q^{-1}\circ p\circ q^{-1}\\
a_6=q\circ p^{-1}\circ q^{-1}\circ p\circ p\circ q^{-1}\\
a_7=q\circ p^{-1}\circ q^{-1}\circ p\circ q\circ p\circ q^{-1}\\
a_8=q\circ p^{-1}\circ q^{-1}\circ p\circ q\circ p^{-1}\circ q\circ p\circ q^{-1}
$$

令 $L=q\circ p^{-1}\circ q^{-1}\circ p$，则 $L^{-1}=p^{-1}\circ q\circ p\circ q^{-1}$。

注意到置换序列有类周期性，$a_{n}=L\circ a_{n-6}\circ L^{-1}$，用快速幂优化即可。

## 鸣谢

1. [Post] [置换和排列-OI Wiki](https://oi-wiki.org/math/permutation/)