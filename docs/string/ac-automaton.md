---
title: AC 自动机
published: 2025-08-25
updated: 2025-09-20
tags: [字符串, 自动机]
category: 信息学
draft: false
---

# AC 自动机

## 引入

KMP 只能有一个模式串，我们构造一个有多个模式串的自动机：

1. 状态：这些串的所有前缀。
2. 失配指针：对于每个节点，$fail$ 指向节点所代表前缀，代表**它**最长且匹配某个前缀（可以为其它串的）的**真后缀**的节点（如果有）。
3. 边：对于每个节点，$next_c$ 表示向节点所代表前缀，加入字符 $c$ 后，其最长的匹配某个前缀的节点（如果有）。

状态是所有前缀，因此可以用一个 Trie 表示。（KMP 可以看作 Trie 是一条链的特殊情况。）

## 构造

可以考虑用 **KMP 的思想**，扩展到一棵 Trie 树的情况。

按长度从小到大考虑每个前缀，对于一个前缀 $S[1\sim i]$，从父亲节点（不包括）开始向上跳 $fail$，直到某个前缀能够扩展出 $S[i]$。

实现可以用拓扑，复杂度 $O(n|Σ|)$。

## 应用

### 多模式串匹配

#### P2414 [NOI2011] 阿狸的打字机

看完之后发现自己并未学会 AC 自动机。

对于询问 $(x,y)$，即为求有多少个串 $y$ 的前缀是，或者在 fail 上直接或间接指向串 $x$ 的结尾。

我们可以对 fail 树（非 Trie 树）上串 $y$ 的每个前缀节点加 $1$，在 $y$ 时对 fail 树上 $ed_x$ 子树求和。

使用树状数组优化。

注意求 fail 树 dfn 序时遍历fail 树，对串 $y$ 的每个前缀加时遍历 Trie 树。

弱化版有P3966 [TJOI2013] 单词、P5357 【模板】AC 自动机。

```cpp {35,44}
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+5;
int len,n,m,cnt,fa[N],ed[N],L[N],R[N],dfn[N],sr[N],dfn_cnt,ans[N];
char s[N];
int ch[N][26],tmpch[N][26],fail[N];
vector<int>tl[N],G[N];
struct Nd{
	int x,y,id;
}Qr[N];
struct BIT{
	int S[N];
	void Upd(int x,int v) {
		for(;x<=dfn_cnt;x+=x&-x) S[x]+=v;
	}
	int Qry(int x,int res=0) {
		for(;x;x-=x&-x) res+=S[x];
		return res;
	}
}B1;
void Build() {
	queue<int>q;
	for(int i=0;i<26;i++) if(ch[0][i]) q.push(ch[0][i]);
	while(!q.empty()) {
		int u=q.front();q.pop();
		for(int i=0;i<26;i++)
			if(ch[u][i]) {
				q.push(ch[u][i]);
				fail[ch[u][i]]=ch[fail[u]][i];
			} else ch[u][i]=ch[fail[u]][i];
	}
}
void Dfs1(int x) {
	dfn[x]=++dfn_cnt;
	for(int v:G[x])
		Dfs1(v);
	sr[x]=dfn_cnt;
}
void Dfs2(int x) {
	B1.Upd(dfn[x],1);
	for(int i:tl[x])
		for(int j=L[i];j<=R[i];j++)
			ans[Qr[j].id]=B1.Qry(sr[ed[Qr[j].x]])-B1.Qry(dfn[ed[Qr[j].x]]-1);
	for(int i=0;i<26;i++)
		if(tmpch[x][i]) Dfs2(tmpch[x][i]);
	B1.Upd(dfn[x],-1); 
}
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>s;
	len=strlen(s);
	int now=0;
	for(int i=0;i<len;i++)
		if(s[i]=='B') {
			now=fa[now];
		} else if(s[i]=='P') {
			ed[++n]=now;
			tl[now].push_back(n);
		} else {
			int x=s[i]-'a';
			if(!ch[now][x]) ch[now][x]=++cnt,fa[cnt]=now;
			now=ch[now][x];
		}
	for(int i=0;i<=cnt;i++)
		for(int j=0;j<26;j++)
			tmpch[i][j]=ch[i][j];
	Build();
	
	cin>>m;
	for(int i=1;i<=m;i++) {
		cin>>Qr[i].x>>Qr[i].y;
		Qr[i].id=i;
	}
	sort(Qr+1,Qr+m+1,[](const Nd &x,const Nd &y) {
		return x.y<y.y;
	});
	for(int i=1,r;i<=m;i=r+1) {
		r=i;
		while(r<m&&Qr[i].y==Qr[r+1].y) ++r;
		L[Qr[i].y]=i;
		R[Qr[i].y]=r;
	}
	
	for(int i=1;i<=cnt;i++) G[fail[i]].push_back(i);
	Dfs1(0);
	Dfs2(0);
	for(int i=1;i<=m;i++) cout<<ans[i]<<"\n";
	return 0; 
}
```

#### P5840 [COCI 2014/2015 #5] Divljak

结合了 AC 自动机与树论。
给 $T$ 中一个串代表每个前缀的节点 $p_i$ fail 树上到根的链加 $1$。但是发现一个串可以出现多次，于是被加的点的集合，应该是 fail 树上到根的链求并。

那么我们将 $p$ 按 dfn 序排序，给 $p_i$ 到根节点的链加 $1$，给 $\text{lca}(p_{i-1},p_i)$ 到根节点的链减 $1$。

> PS：这个过程让我想起了建虚树的过程。
>
> 以三个点为例：都重叠的部分会被加 $3$ 次，减 $2$ 次；两两重叠的部分会被加 $2$ 次，减 $1$ 次……

**路径加+单点查**，可以用**树上差分**转为**单点加+区间查**。

#### CF587F Duff is Mad

令 $m=\sum_{i=1}^n|s_i|$。

根号分治，设阈值为 $T$：

1. $|s_k|>T$，这样的块不超过 $\frac{m}{T}$ 个，直接给 AC 自动机 fail 树上 $s_k$ 所有前缀节点到根加 $1$（这个可以$O(m)$ dfs 对子树内求和），复杂度 $O(\frac{m^2}{T})$。
2. $|s_k|\leq T$，离线下来，做扫描线在 $l-1$ 和 $r$ 计算贡献，给 fail 树上 $s_i$ 的结束节点的子树加 $1$，遍历 $s_k$ 的前缀节点求和（dfs 序列上区间加+单点查，树状数组实现），复杂度 $(qT\log m+n\log m)$。

> PS：我之前看到扫描线求图形并的周长和面积，就以为扫描线就仅指这俩，事实上有很多离线二维问题，都是用扫描线去扫一维，数据结构等维护另一维。

### AC 自动机上 DP

不禁想起了 **DP 套 DP**（狭义），但那个是内层用 DP 求出 DFA 的状态，外层用 DP 求解答案。

一般设 $f_{i,j}$ 为长度 $i$ 状态 $j$ 的方案数。

有些题由于转移比较简单，可使用矩阵快速幂优化。

#### P4052 [JSOI2007] 文本生成器

比较套路的板题。

# 鸣谢

1. [Slide] 字符串-张隽恺-2025