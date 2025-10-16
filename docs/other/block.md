---
title: 分块好题（一）
published: 2025-09-20
updated: 2025-09-20
tags: [分块, 题集]
category: 信息学
draft: false
---

#### P8264 [Ynoi Easy Round 2020] TEST_100

设值域为 $[l,r]$，对于每次操作 $f_i(x)=|x-a_i|$：

1. $a_i\le l$ 则有 $f_i(x)=x-a_i$。
2. $a_i\ge r$ 则有 $f_i(x)=a_i-x$。
3. 否则 $l<a_i<r$，我们发现是将关于 $x=a_i$ 对称的 $x$ 的答案合并到一起，把少的半边往大的半边合并即可（可以用并查集）。

分块解决，将每个块对应的区间的函数复合。

细节很有思维含量，拼尽全力终于过题（当然也有别的写法，没去看了）。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+5;
int n,m,a[N],block,cnt;
int V=1e5;
int L[N],R[N];
int fa[N];
int f[330][N];
int Find(int x) {
	return x==fa[x]?x:fa[x]=Find(fa[x]);
}
void Merge(int x,int y) {
	fa[Find(y)]=Find(x);
}
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>n>>m;
	cnt=block=sqrt(n);
	for(int i=1;i<=cnt;i++) L[i]=(i-1)*block+1,R[i]=i*block;
	if(R[cnt]<n) {
		++cnt;
		L[cnt]=R[cnt-1]+1;
		R[cnt]=n;
	}
	for(int i=1;i<=n;i++) cin>>a[i];
	for(int i=1;i<=cnt;i++) {
		for(int j=0;j<=V;j++) fa[j]=j;
		
		int k=1,b=0,l=0,r=V;
		for(int j=L[i];j<=R[i];j++) {
			int pos=(a[j]-b)/k;//满足 kt+b=a_j 的 t，也即 x=t 为对称轴
			if(pos*2<l+r) {
				for(int x=l;x<pos;x++) Merge(pos*2-x,x);
				l=max(l,pos);
				if(k==-1) k=1,b=a[j]-b;//后半段 <= a_j，a_j-(kx+b)
				else k=1,b=b-a[j];//后半段 >= a_j，(kx+b)-a_j
			} else {
				for(int x=pos+1;x<=r;x++) Merge(pos*2-x,x);
				r=min(r,pos);
				if(k==-1) k=-1,b=b-a[j];//前半段 >= a_j，(kx+b)-a_j
				else k=-1,b=a[j]-b;//前半段 <= a_j,a_j-(kx+b)
			}
		}
		for(int j=0;j<=V;j++) f[i][j]=k*Find(j)+b;
	}
	int lastans=0,l,r,v;
	while(m--) {
		cin>>l>>r>>v;
		l^=lastans,r^=lastans,v^=lastans;
		int bl=(l-1)/block+1,br=(r-1)/block+1;
		if(bl==br) {
			for(int i=l;i<=r;i++) v=abs(v-a[i]);
		} else {
			for(int i=l;i<=R[bl];i++) v=abs(v-a[i]);
			for(int i=bl+1;i<br;i++) v=f[i][v];
			for(int i=L[br];i<=r;i++) v=abs(v-a[i]);
		}
		lastans=v;
		cout<<v<<"\n";
	}
	return 0;
}
```