---
title: 树分治
published: 2025-08-16
updated: 2025-09-20
tags: [树上问题, 分治]
category: 信息学
draft: false 
---

# 点分治

## 基本思想

将路径分为经过 $u$ 和不经过 $u$ 的两类，在每次分治中计算经过 $u$ 的路径数量。

## 统计方法

1. 一般：遍历 $u$ 的每个子节点 $v$，把 $v$ 子树内的节点记录下来，得到答案并更新数组。
2. 容斥：把 $u$ 子树内的节点都记录下来排序，双指针得到的 $u$ 子树内点对数量，减去每个子节点 $v$ 子树内的点对数量，即为经过 $u$ 的路径的答案。*

## 复杂度

每次 $O(n)$ 寻找重心，子树大小降为最大 $\lfloor size/2 \rfloor$，大概 $\log(n)$ 层。每层遍历大概 $n$ 个节点，复杂度上限 $O(n\log(n))$。

## 用途

统计树上满足条件的路径（点对）个数（或点权和等），一般条件和距离相关。

```cpp
void findrt(int u,int f){
	siz[u]=1;
	maxp[u]=0;
	for(int i=fir[u];i;i=nex[i]){
		int v=to[i];
		if(vis[v]||v==f) continue;
		findrt(v,u);
		siz[u]+=siz[v];
		maxp[u]=max(maxp[u],siz[v]);
	}
	maxp[u]=max(maxp[u],sum-siz[u]);
	if(maxp[u]<maxp[rt]) rt=u;
}
void dfz(int u){
	vis[u]=1;
	
	//计算！！！
	
	for(int i=fir[u];i;i=nex[i]){
		int v=to[i];
		if(vis[v]) continue;
		rt=0;
		sum=siz[v];
		findrt(v,u);
		dfz(rt);
	}
}
```

# 边分治

其实和点分治差不多。

# 点分树

点分树又被成为动态点分治。

## 基本思想

通过点分治的方法递归，每次的得到 $v$ 子树内的重心与 $u$ 相连，建出重构树。

## 特殊性质

1. 最多 $log(n)$ 层。
2. 重构树上的 LCA(x,y) 必定在原树 x 到 y 的路径上，有 $dis_{x,y}=dis_{x,LCA(x,y)}+dis_{y,LCA(x,y)}$。*

点分树和点分治一样适用于统计满足条件的路径的点权和，但点分树可修改点权，更加灵活，保证了每次只需查询或修改 $log(n)$ 层。

## 具体实现

1. 得到重构树，其实只需记录 $fa_u$。
2. 一个显然的思路是在重构树中将 $now$ 不断变为父亲节点，得到在 $fa_{now}$ 子树内但不在 $now$ 子树内的贡献。
3. 想想如何更新。我们用 $f_i$ 存 $i$ 子树中 $i$ 的数据，$g_i$ 存 $i$ 子树内 $fa_i$ 的数据。


#### [P6329 【模板】点分树](https://www.luogu.com.cn/problem/P6329)

```cpp
void modify(int x,int k){
	int now=x;
	while(now){
		F.modify(now,getdis(now,x),k);
		if(Fa[now]) G.modify(now,getdis(Fa[now],x),k);
		now=Fa[now];
	}
}
int query(int x,int k){
	int now=x,pre=0,ret=0;
	while(now){
		int t=getdis(now,x);
		if(t>k){
			pre=now,now=Fa[now];
			continue;
		}
		ret+=F.ask(now,k-t);
		if(pre) ret-=G.ask(pre,k-t);
		pre=now,now=Fa[now]; 
	}
	return ret;
}
```