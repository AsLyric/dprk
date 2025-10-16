---
title: 前缀数组与 KMP 算法
published: 2025-08-25
updated: 2025-09-20
tags: [字符串, KMP]
category: 信息学
draft: false
---

# 前缀数组

有一些博客和题解没有把这个归为单独的知识点，直接视为 KMP 额外得到的信息。

$next_i$ 为 $S[1\sim i]$ 的真前缀和真后缀匹配的最大长度（$next$ 即为前缀数组）。

## 求解

设 $S[1\sim i]$ 所有真前缀和真后缀的匹配为 $M_i=\{S[1\sim p_{i,1}],S[1\sim p_{i,2}],\dots,S[1\sim p_{i,k}]\}$。

如果 $S[i]=S[p_{i-1,x}+1]$，那么 $S[1\sim p_{i-1,x}+1]\in M_i$。可以发现 $M_i$ 中的串只能为 $M_{i-1}$ 中的串或者空串接上 $S[i]$。

注意到 $M_i=M_{next_i}\cup \{S[1\sim next_i]\}$，也就是说 $i$ 一直往 $next_i$ 跳就遍历了每种 $S[1\sim i]$ 的真前缀和真后缀的匹配。

> PS：$next$ 数组即为 KMP 算法中的失配数组（$fail$ 数组），让每个 $i$ 与 $next_i$ 连边，构成了一颗以 $0$ 为根的失配树（fail 树）。

那么求解 $next_i$ 的过程，令 $j=next_{i-1}$，一直往 $next_j$ 跳，直到 $S[i]=S[next_j+1]$ 或者 $j=0$（此时 $next_i$ 也只能为 $0$）。

```cpp
for(int i=2,j=0;i<=m;i++){
    while(j>0&&s[i]!=s[j+1]) j=nex[j];
    if(s[i]==s[j+1]) ++j;
    nex[i]=j;
}
```

## 复杂度

发现 $next_i\leq next_{i-1}+1$，所以 $j$ 最多向右移动 $n$ 次。

记势能为 $next_i$，每次向上跳时势能减少，只在扩展时增加势能。因此复杂度为 $O(n)$，常数极小。

## 应用

### border

#### CF432D Prefixes and Suffixes

这题解法非常多，但还是前缀数组非常好用。

### 字符串周期

所有周期一定是最小周期的整数倍。（假设不是整数倍，可以得到更小的周期。）

#### P7114 [NOIP2020] 字符串匹配

计数部分有点困难。

若 $next_n>0$ 且 $n\bmod (n-next_n)=0$ ，则 $s[1,n]$ 是由相同的串拼成的。

#### P4391 [BOI2009] Radio Transmission 无线传输

$n-next_n$ 就是字符串的最短周期。

#### P3435 [POI2006] OKR-Periods of Words

要使周期最大，应该让 $next_i$ 尽量往前跳。

# KMP

匹配串进行匹配的过程和 $next$ 数组的求解类似。

#### P3375 【模板】KMP

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e6+5;
int n,m,nex[N];
char s[N],t[N];
int main(){
	scanf("%s%s",s+1,t+1);
	n=strlen(s+1),m=strlen(t+1);
	for(int i=2,j=0;i<=m;i++){
		while(j>0&&t[i]!=t[j+1]) j=nex[j];
		if(t[i]==t[j+1]) ++j;
		nex[i]=j;
	}
	for(int i=1,j=0;i<=n;i++){
		while(j>0&&s[i]!=t[j+1]) j=nex[j];
		if(s[i]==t[j+1]) ++j;
		if(j==m){
			printf("%d\n",i-m+1);
		}
	}
	for(int i=1;i<=m;i++) printf("%d ",nex[i]);
}
```

#### P5256 [JSOI2013] 编程作业

问 $S$ 有多少个子串和 $T$ 匹配，这里定义匹配为存在一种重新映射小写字母的方案使得两串相等。

hash 还是可以做，但是这题的 KMP 做法比较特别，需要重载字符的相等函数：

1. 若为大写字母，两字符相等。
2. 若为小写字母，两字符到上一次出现的距离相等，或都是第一次出现。

# 扩展 KMP（Z 算法）

这个 Z 算法是我直译的 **Z algorithm** 哈。

不想学了，直接跳到 AC 自动机部分。

## 鸣谢

1. [Slide] 字符串选讲-周易-2022