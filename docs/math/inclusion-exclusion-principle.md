---
title: 容斥入门
published: 2025-08-15
updated: 2025-10-02
tags: [组合数学]
category: 信息学
draft: false 
---

# 容斥原理

## P2567 [SCOI2010] 幸运数字

预处理幸运号码，显然 $[l,r]$ 区间内是 $x$ 倍数的近似幸运号码数量是 $\lfloor\frac{r}{x}\rfloor-\lfloor\frac{l-1}{x}\rfloor$。

然而两个幸运号码对应的近似幸运号码可能有交集，考虑容斥。

设 $c_i$ 为任意 $i$ 个幸运号码 $\text{lcm}$ 的倍数的数的个数，显然答案为 $c_1-c_2+c_3-c_4+\dots$。

## P6076 [JSOI2015] 染色问题
同时对行、列、颜色有至少的限制。

考虑容斥。

从颜色出发，如果求出了最多使用 $i$ 种颜色的方案数 $f_i$，答案就是：

$$\sum_{i=0}^c(-1)^{c-i}{c \choose i}f_i$$

已经确定了这 $i$ 这种颜色，我们很方便求出某一行某 $j$ 列为空，其它任选且不全为空的方案数：

$$(i+1)^{m-j}-1$$

而要求每一列不为空，我们仍需要容斥：

$$f_i=\sum_{j=0}^m(-1)^j{m \choose j}((i+1)^{m-j}-1)^n$$

## P10597 BZOJ4665 小 w 的喜糖

## CF451E Devu and Flowers

## CF997C Sky Full of Stars
式子好长……

设 $f_{i,j}$ 为钦定 $i$ 个颜色相同行和 $j$ 个颜色相同列的方案数。

答案为 $\sum_{i,j =  0,i+j>0}^n (-1)^{i+j+1}{n \choose i}{n \choose j}f_{i,j}$。

若 $j=0$，则 $f_{i,j}=3^{i+(n-i)*n}$，$j=0$ 同理。

否则 $f_{i,j}=3^{(n-i)*(n-j)+1}$。

但是 $n$ 较大，我们单独来算 $i,j$ 不为 $0$ 的情况：

$$
\begin{align}
\sum_{i=1}^n\sum_{j=1}^n (-1)^{i+j+1}{n \choose i}{n \choose j}3^{(n-i)*(n-j)+1}\\
=-3^{n^2+1}\sum_{i=1}^n (-1)^i{n \choose i}3^{-in}\sum_{j=1}^n{n \choose j}(-3^{-n+i})^j
\end{align}
$$

右边这一堆 $\sum_{j=1}^n{n \choose j}(-3^{-n+i})^j$ 很接近二项式定理化简式子的经典形式。

$$
\begin{align}
=-3^{n^2+1}\sum_{i=1}^n (-1)^i{n \choose i}3^{-in}\sum_{j=1}^n{n \choose j}1^{n-j}(-3^{-n+i})^j\\
=-3^{n^2+1}\sum_{i=1}^n (-1)^i{n \choose i}3^{-in}((\sum_{j=0}^n{n \choose j}1^{n-j}(-3^{-n+i})^j)-1)\\
=-3^{n^2+1}\sum_{i=1}^n (-1)^i{n \choose i}3^{-in}((-3^{i-n}+1)^n-1)
\end{align}
$$

# 广义容斥原理（待补题）
令计数函数 $α(x)$ 表示至少具有 $x$ 个性质的元素个数，$β(x)$ 表示恰好具有 $x$ 个性质的元素个数。

定理（性质总数为 $n$）：
$$β(m)=\sum_{i=0}^{n-m}(-1)^i{m+i \choose m} α(m+i)$$

$$β(0)=\sum_{i=n}^n(-1)^iα(i)$$

# Min-Max 容斥（待补题）

$$
\max(S)=\sum_{T⊆S}(−1)^{|T|+1}\min(T)\\
\min(S)=\sum_{T⊆S}(−1)^{|T|+1}\max(T)
$$

## 证明

设 $S$ 从大到小排序后的数组为 $A_{1,n}$。

考虑每个数的贡献（若有相同的数，我们钦定最前面一个贡献）：

1. $i=1$，只有它一个在集合 $T$ 中时有贡献，贡献为 $A_i$。
2. $i>1$，$i$ 在 $T$ 中，$1\sim i-1$ 可在可不在，集合大小为奇数、偶数的方案数分别是 $2^{i-2}$，贡献为 $0$。

## 在期望下

$$
E(\max(S))= \sum_{T⊆S}(−1)^{|T|+1}E(\min(T))\\
E(\min(S))= \sum_{T⊆S}(−1)^{|T|+1}E(\max(T))
$$

#### QOJ12405 Path Killer

https://blog.csdn.net/weixin_44059127/article/details/112971085

设 $S$ 为边被选中的期望次数的集合，答案 $E(\max(S))=\sum_{T\subset S}(-1)^{|T|+1}E(\min(T))$。

设 $T$ 对应的边集覆盖了 $x$ 个点，由期望方程易得 $E(\min(T))=\frac{n}{x}$。

DP，设 $f_{i,j,k,0/1}$ 为 $i$ 子树内覆盖了 $j$ 个点（不含自己），到根覆盖了 $k$ 个点，集合大小奇数/偶数的方案数。

奇偶这一维可以很巧妙地消掉，只要 DP 时让集合大小奇数的系数为负，集合大小偶数的系数为正，“奇 $+$ 奇 $=$ 偶”对应“负 $\times$ 负 $=$ 正”等。