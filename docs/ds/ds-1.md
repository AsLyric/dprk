#### P7476 「C.E.L.U-02」苦涩

简单题，模拟赛时应该能想出来的，只是不会证明复杂度：

最多产生 $m\log n$ 个区间，删除每个区间最多用 $\log n$ 次递归，所以复杂度为 $O(m \log^2 n)$。

#### P3792 由乃与大母神原型和偶像崇拜

简单题。

#### P5278 算术天才⑨与等差数列

转化为充要条件：

1. $max_{i=l}^ra_i-min_{i=l}^ra_i=k(r-l)$。
2. $gcd_{i=l}^{r-1} a_{i+1}-a_i=k$。
3. 区间没有重复数字。

#### P4211 [LNOI2014] LCA

考虑离线下来，在 $l-1$ 和 $r$ 计算贡献。

$LCA(i,z)$ 为 $z$ 到根节点遇到的第一个 $i$ 的祖先。

那么对于 $i$，我们将 $i$ 到根的值都加 $1$。对于询问 $z$ 直接求 $z$ 到根的和即可。

树链剖分模板。

#### P2824 [HEOI2016/TJOI2016] 排序

很经典的 trick，二分答案 $x$，$\ge x$ 的改为 $1$，$<x$ 的改为 $0$，就很好区间排序了（询问区间中 $1$ 的个数）。

#### P4219 [BJOI2014] 大融合

https://www.luogu.com.cn/article/cdrlflnf

不难看出需要支持单点查询和给一条链加上一个数。

而链加可以给底下的节点加，上边的节点的父亲减，查询就变成了求子树和。

#### CF765F Souvenirs

好题，有线段树、分块、莫队等多种解法。

#### GYM104337D Darkness II

jly 给的神题。

题目和时间无关，本质上是矩形的合并，而合并是 $O(n)$ 次的。

设一个矩形为 $(xl,xr,yl,yr)$，另一个矩形能与它合并，当且仅当与 $(xl-2,xr+2,yl,yr)$ 或 $(xl-1,xr+1,yl-1,yr+1)$ 或 $(xl,xr,yl-2,yr+2)$ 的区域有交。

那么我们每加入一个矩形，分别查询和这些区域有交的矩形，合并为新的矩形，直到找不到。

设查询为 $(a,b,c,d)$。

我们思考一下如何解决 $x$ 维有交，使用线段树：$[a,b]$ 完全包含线段树节点 $[l,r]$ 时，$x$ 轴上区间和 $[l,r]$ 有交的任意矩形，都和 $[a,b]$ 有交。

这样不会遗漏，但有一个弊端，就是更新时需要递归所有线段树上，和它相交的区间的节点 ，$O(V)$ 级别，会超时。

我们需要寻求一种方法，能让查询和更新，都适时结束递归[^1]，发现更新如果适时结束递归，无法更新的是它的区间完全包含的 $[l,r]$。

可以打个懒标记并进行标记永久化，也就是只要查询时，访问到当前线段树区间 $[l,r]$，就用完全包含 $[l,r]$ 的矩形，尝试更新答案。

线段树上每一个节点，用一个 `vector` 记录 $x$ 上区间完全包含 $[l,r]$ 的矩形，另一个 `vector` 记录和 $[l,r]$ 有交，但并非完全包含 $[l,r]$ 的矩形。

对于 $y$ 维有交，我们在预处理时，将最初的黑色格点按 $y$ 从小到大排序，这样在线段树上每个节点存储的矩形，$yl$ 都是递增且 $\le d$ 的，判断节点最后一个矩形的 $yr$ 是否 $\ge c$ 即可。

```cpp
#include<bits/stdc++.h>
#define ll long long
#define ls k<<1
#define rs k<<1|1 
using namespace std;
const int N=1e5+5;
int n,tot;
int p[N*5],del[N];
ll ans;
struct Nd{
	int xl,xr,yl,yr;
}a[N];
vector<int>V1[N*4],V2[N*4];
void Modify(int k,int l,int r,int x,int y,int id) {
	if(x<=l&&r<=y) {
		V1[k].push_back(id);//完全包含 [l,r] 的
		return;
	}
	V2[k].push_back(id);//和 [l,r] 有交的
	int mid=l+r>>1;
	if(x<=mid) Modify(ls,l,mid,x,y,id);
	if(y>mid) Modify(rs,mid+1,r,x,y,id);
}
int Qry(int k,int l,int r,int x,int y,int val) {
	while(V1[k].size()&&del[V1[k].back()]) V1[k].pop_back();
	if(V1[k].size()&&a[V1[k].back()].yr>=val) return V1[k].back();
	if(x<=l&&r<=y) {
		while(V2[k].size()&&del[V2[k].back()]) V2[k].pop_back();
		if(V2[k].size()&&a[V2[k].back()].yr>=val) return V2[k].back();
		return 0;
	}
	int mid=l+r>>1,ans=0;
	if(x<=mid) ans=Qry(ls,l,mid,x,y,val);
	if(ans) return ans;
	if(y>mid) ans=Qry(rs,mid+1,r,x,y,val);
	return ans;
}
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>n;
	for(int i=1;i<=n;i++) {
		cin>>a[i].xl>>a[i].yl;
		a[i].yr=a[i].yl;
		p[++tot]=a[i].xl-2;
		p[++tot]=a[i].xl-1;
		p[++tot]=a[i].xl;
		p[++tot]=a[i].xl+1;
		p[++tot]=a[i].xl+2;
	}
	sort(p+1,p+tot+1);
	tot=unique(p+1,p+tot+1)-p-1;
	sort(a+1,a+n+1,[](const Nd &x,const Nd &y) {
		return x.yl<y.yl;
	});
	for(int i=1;i<=n;i++) {
		a[i].xl=a[i].xr=lower_bound(p+1,p+tot+1,a[i].xl)-p;
		int x=0;
		while((x=Qry(1,1,tot,a[i].xl,a[i].xr,a[i].yl-2))||(x=Qry(1,1,tot,a[i].xl-1,a[i].xr+1,a[i].yl-1))||(x=Qry(1,1,tot,a[i].xl-2,a[i].xr+2,a[i].yl))) {
			del[x]=1;
			a[i].xl=min(a[i].xl,a[x].xl);
			a[i].xr=max(a[i].xr,a[x].xr);
			a[i].yl=min(a[i].yl,a[x].yl);
		}
		Modify(1,1,tot,a[i].xl,a[i].xr,i);
	}
	for(int i=1;i<=n;i++)
		if(!del[i]) ans+=1ll*(p[a[i].xr]-p[a[i].xl]+1)*(a[i].yr-a[i].yl+1);
	cout<<ans;
	return 0;
}
```

[^1]: 指 $[a,b]$ 完全包含 $[l,r]$ 就结束递归，这样只需访问线段树 $O(\log V)$ 个节点。

#### 鸣谢

1. [Slide] 线段树，树状数组-李欣隆-2022（部分题目）
2. [Slide] 数据结构题目选讲-蒋凌宇-2023（部分题目）