---
title: 反悔贪心
published: 2025-09-19
updated: 2025-09-20
tags: [贪心]
category: 信息学
draft: false
---

## 反悔贪心

今天（25/08/29）看到一道反悔贪心不会做，然后被嘲讽了，于是发现好像做过很多遍（上上周才做过），原来我的记忆力这么差的吗……

#### P4053 [JSOI2007] 建筑抢修 & P3545 [POI 2012] HUR-Warehouse Store

水。

#### P3620 [APIO/CTSC2007] 数据备份

https://www.luogu.com.cn/article/lg37ze54

抽象为从从 $n-1$ 个点中选 $m$ 条，不能选相邻的点。

我们贪心地使用一个优先队列来存储所有可能的点，每次取出最小的。

我们使用一个反悔机制：每次取出一个点，给优先队列再加入一个点代表它左边的点到右边的点这一段（权值为左边的点的权值 $+$ 右边的点的权值 $-$ 自身的权值），并更新左右的点。

有种容斥一样巧妙的感觉。

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5;
int n,K,lst[N],nxt[N];
bool vis[N];
ll a[N],ans;
priority_queue<pair<ll,int> >Q;
void Solve(int x) {
	vis[lst[x]]=1;
	vis[nxt[x]]=1;
	a[x]=a[lst[x]]+a[nxt[x]]-a[x];
	lst[x]=lst[lst[x]];
	nxt[x]=nxt[nxt[x]];
	nxt[lst[x]]=x;
	lst[nxt[x]]=x;
}
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>n>>K;
	for(int i=0;i<n;i++) cin>>a[i];
	for(int i=n-1;i;i--) {
		a[i]-=a[i-1];
		lst[i]=i-1;
		nxt[i]=i+1;
		Q.push({-a[i],i});
	}
	a[0]=a[n]=1e18;
	while(K--) {
		while(vis[Q.top().second]) Q.pop();
		int u=Q.top().second;
		Q.pop();
		ans+=a[u];
		Solve(u);
		Q.push({-a[u],u});
	}
	cout<<ans;
	return 0;
}
```

#### P1792 [国家集训队] 种树

破环为链，$1$ 种那么 $n$ 不能种，否则 $1$ 不种 $n$ 任意。

看了一眼题解，原来并不用？

#### P7219 [JOISC2020] 星座 3

每次模拟赛考 JOI 的题都要被虐爆，但这个题真的很巧妙。

我们从下往上遍历每行，某一列从小白船变成空格，相当于把左右给联通起来，可以用并查集维护。

而每次加入一颗星星，要么将它涂黑，要么将影响它的一堆星星涂黑，用数据结构维护涂黑每一列影响范围内的星星的代价。

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=2e5+5;
int n,m;
vector<int>upd[N];
vector<pair<int,int> >nd[N];
ll S[N],ans;
struct DSU {
	int fa[N];
	int Find(int x) {
		return fa[x]==x?x:fa[x]=Find(fa[x]);
	}
}L,R;
void Add(int x,ll v) {
	for(;x<=n;x+=x&-x) S[x]+=v;
}
ll Qry(int x) {
	ll ret=0;
	for(;x;x-=x&-x) ret+=S[x];
	return ret;
}
int main() {
	scanf("%d",&n);
	for(int i=1,x;i<=n;i++) scanf("%d",&x),upd[x].push_back(i);
	for(int i=1;i<=n+1;i++) L.fa[i]=R.fa[i]=i;
	scanf("%d",&m);
	for(int i=1,x,y,c;i<=m;i++) {
		scanf("%d%d%d",&x,&y,&c);
		nd[y].push_back({x,c});
	}
	ll t;
	for(int i=1;i<=n;i++) {
		for(pair<int,int> j:nd[i]) {
			if((t=Qry(j.first))>=j.second) {
				ans+=j.second;
			} else {
				ans+=t;
				Add(L.Find(j.first)+1,j.second-t);
				Add(R.Find(j.first),t-j.second);
			}
		}
		for(int j:upd[i]) {
			L.fa[j]=j-1;
			R.fa[j]=j+1;
		}
	}
	printf("%lld",ans);
	return 0;
}
```