---
title: 思维好题（一）
published: 2025-09-20
updated: 2025-09-20
tags: [Ad-hoc]
category: 信息学
draft: false
---

# 思维杂题

#### 简单题

给定一个 $n$ 个点 $m$ 条边的有向图，边有边权，求从每个点出发的边权严格递增的最长路径长度。

做法非常反套路，我们甚至不需要建图，直接按边权从大到小，用边终点的答案更新起点的答案。

没有查到这道题，但有一个类似题 GYM364616C。

```cpp
#include<bits/stdc++.h>
#define fi first
#define se second
using namespace std;
const int N=5e5+5,V=1e5;
int n,m,ans;
int f[N],g[N];
vector<pair<int,int> > e[N];
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>n>>m;
	for(int i=1,u,v,w;i<=m;i++) {
		cin>>u>>v>>w;
		e[w].push_back({u,v});
	}
	for(int i=V;i;i--) if(e[i].size()) {
		for(auto v:e[i]) g[v.se]=f[v.se];
		for(auto v:e[i]) f[v.fi]=max(f[v.fi],g[v.se]+1);
	}
	for(int i=1;i<=n;i++) ans=max(ans,f[i]);
	cout<<ans;
	return 0;
} 
```

#### P9525 [JOISC 2022] 团队竞技

https://www.luogu.com.cn/article/3a6dlfrr

维护优先队列，如果某一个数有两项都是剩下中最大的，那肯定不能选，一直 `pop` 直到三个队顶都可选。

#### P8162 [JOI 2022 Final] 让我们赢得选举 / Let's Win the Election

https://www.luogu.com.cn/article/8p4ezfkt

分别考虑反向关键边（反向后影响最短路）和非关键边后的答案。
 
对于非关键边，答案为 $D+min(dis_{1,n},dis_{1,v}+C+dis_{v,n})+min(dis_{n,1},dis_{n,u}+C+dis_{v,1})$。 

因此定义关键边为不在 $1$ 为起点、$1$ 为终点、$n$ 为起点、$n$ 为终点的任意一颗最短路树上的边，仅有 $O(n)$ 条。

对于关键边，暴力反向并求 $dis$ 即可。 

略有细节。

#### UOJ958 【USsR #1】彩色括号

易得括号序列合法的充要条件：

1. 每种颜色的左右括号数量相等。
2. 去掉每种颜色后的前缀和分别非负。

我们不妨先把每种颜色左右括号数量改到平衡（左括号多则优先改最右边的左括号，右括号同理），这样一定不劣。

然后三种颜色还需改的话，一定是改一对最左边的 `)` 和最右边的 `(`，设三种颜色改的数量是 $x_1,x_2,x_3$。

对颜色 $i$ 的前缀 $j$ 来说，增加了 $\min(x_i,T_{i,j})$，其中 $T_{i,j}$ 为颜色 $i$，前缀 $j$ 的 `)` 数量和后缀 $j$ 的 `(` 数量的最小值。

条件 $2$ 转化为形如 $C_{i,k}+C_{j,k}+\min(x_i,T_{i,k})+\min(x_j,T_{j,k})\ge 0$，其中 $C_{i,k}$ 为颜色 $i$ 前缀 $k$ 的和。

再转化为：
$$
\left\{\begin{matrix}
C_{i,k}+C_{j,k}+x_i+x_j\ge 0\\
C_{i,k}+C_{j,k}+x_i+T_{j,k}\ge 0\\
C_{i,k}+C_{j,k}+T_{i,k}+x_j\ge 0
\end{matrix}\right.
$$
因为只有三种颜色，我们可以枚举 $x_1$，求出 $x_2,x_3$ 的取值范围，给需修改的括号数取 $\min$ 即可。