---
title: bitset 求解高维偏序问题
published: 2025-09-17
updated: 2025-09-20
tags: [STL, 偏序]
category: 信息学
draft: false 
---

#### QOJ6873 King's Ruins

五维偏序。

第一次学习 bitset 解决高维偏序，还挺简单的。$a$ 能偏序 $b$ 当且仅当每一维度都能偏序 $b$。每一维度上排序一下，求得能偏序哪些，总的取交即可。

复杂度 $O(\frac{n^2k}{w})$。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=5e4+5;
int T,n,val[N][6],id[N];
bitset<N> f[N];
void Solve() {
	cin>>n;
	for(int i=1;i<=n;i++)
		for(int j=0;j<6;j++) cin>>val[i][j];
	for(int i=1;i<=n;i++) id[i]=i,f[i].set();
	for(int i=0;i<5;i++) {
		sort(id+1,id+n+1,[&](const int &x,const int &y) {
			return val[x][i]<val[y][i];
		});
		bitset<N>tmp;
		tmp.reset();
		for(int l=1,r=0;l<=n;l=r+1) {
			//bug：while(r<n&&val[l][i]==val[r+1][i]) {
			while(r<n&&val[id[l]][i]==val[id[r+1]][i]) {
				++r;
				tmp[id[r]]=1;
			}
			for(int j=l;j<=r;j++)
				f[id[j]]&=tmp;
		}
	}
	for(int i=1;i<=n;i++) {
		int mx=0;
		for(int j=1;j<i;j++)
			if(f[i][j]) mx=max(mx,val[j][5]);
		val[i][5]+=mx;
		cout<<val[i][5]<<"\n";
	}
}
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>T;
	while(T--) Solve();
}
```

#### P3810 【模板】三维偏序（陌上花开）

https://www.luogu.com.cn/article/gtnbcf0u

空间开不下，可以分组求。