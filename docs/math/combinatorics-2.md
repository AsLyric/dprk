---
title: 组合计数好题（一）
published: 2025-08-19
updated: 2025-09-29
tags: [DP, 数学, 组合数学, 题集]
category: 信息学
draft: false 
---

包含一些 DP 的题目，比较套路的转化用 $\fcolorbox{red}{while}{red block}$ 标出。

#### CF1327F AND Segments

https://www.luogu.com.cn/article/fjagr4ee

二进制下每一位之间是独立的，拆开来分析。

对于为 $1$ 的要求，只能全填 $1$，对于为 $0$ 的要求就非常麻烦了。

尝试 ~~查看题解~~ 设 $f_i$ 为以 $i$ 为最后一个 $0$ 的答案，观察哪些可以进行转移，发现会是一个区间，我们需要求 $i$  前面最后一个 $0$ 至少在的位置 $pos_i$，这个是单调不降的，就能线性转移了。

#### CF1716F Bags with Balls
##### 补充：常幂转下降幂（第二类斯特林数）

$$n^m=\sum_{i=0}^m\begin{Bmatrix}m\\i\end{Bmatrix}{n \choose i}i!$$

> 这个可以用组合意义理解，$n^m$ 为 $m$ 步，每步有 $n$ 种路的方法数。随意选 $i$ 种路（${n\choose i}i!$），再把 $m$ 个分到里面（$\begin{Bmatrix}m \\i\end{Bmatrix}$，按照第二类斯特林数的定义，$m$ 步有区分，$n$ 种路无区分且至少有 $1$ 个）。
>
> 顺便一提，第二类斯特林数的通项公式，是由这个反演得到的。

设 $1 \sim m$ 奇数 $x$ 个，偶数 $y$ 个：

$
\begin{aligned}
ans& =\sum_{i=0}^{n}\fcolorbox{red}{white}{\(i^k\)} \binom{n}{i} x^iy^{n-i}\end{aligned}$

由于 $n$ 巨大，我们试着转换枚举的变量：

$\begin{aligned}ans& =\sum_{i=0}^{n}\binom{n}{i} x^iy^{n-i} \sum_{j=0}^k \begin{Bmatrix} k\\j \end{Bmatrix} \binom{i}{j}j!\\
& = \sum_{j=0}^{k}\begin{Bmatrix} k\\j \end{Bmatrix} \sum_{i=0}^{n} \frac{n!}{i!(n-i)!} x^iy^{n-i} \frac{i!}{(i-j)!} \\
& = \sum_{j=0}^{k}\begin{Bmatrix} k\\j \end{Bmatrix} \sum_{i=0}^{n} \fcolorbox{red}{white}{\(\frac{n!}{(n-i)!(i-j)!}\)} x^iy^{n-i}\end{aligned}$

加红的这一部分看起来可以化简（分子、分母同乘 $(n-j)!$）：

$
\begin{aligned}
ans&= \sum_{j=0}^{k}\begin{Bmatrix} k\\j \end{Bmatrix} \frac{n!}{(n-j)!}\fcolorbox{red}{white}{\(\sum_{i=0}^{n}\binom{n-j}{n-i} x^iy^{n-i}\)}\end{aligned}$

加红的这一部分我们往二项式定理的方向化：

$\begin{aligned}ans& = \sum_{j=0}^{k}\begin{Bmatrix} k\\j \end{Bmatrix} \frac{n!}{(n-j)!}\sum_{t=0}^{n-j}\binom{n-j}{t} x^{n-t}y^{t}\\& = \sum_{j=0}^{k}\begin{Bmatrix} k\\j \end{Bmatrix} n^{\underline j}x^j\sum_{t=0}^{n-j}\binom{n-j}{t} x^{n-j-t}y^{t}\\
& = \sum_{j=0}^{k}\begin{Bmatrix} k\\j \end{Bmatrix} n^{\underline j}x^jm^{n-j}
\end{aligned}$

由于 $k\leq 2000$，随便写个预处理斯特林数什么的就行了。

#### P5664 [CSP-S2019] Emiya 家今天的饭（不合法元素唯一）

“每种主要食材至多在一半的菜（即 $\lfloor \frac{k}{2}\rfloor$ 道菜）中被使用”这个限制较为复杂，但注意到被使用 $\geq \lfloor \frac{k}{2}\rfloor+1$ 次的至多只有一种主要食材。用满足其它条件的总方案数减去满足其它条件但某一列打破限制的方案数。

#### AGC052C Nondivisible Prefix Sums（不合法元素唯一）

https://www.luogu.com.cn/article/1zbq1bhy

https://www.luogu.com.cn/article/83tecnmo

如果数列总和不为 $P$ 倍数，不合法一定是因为严格众数出现过多（若几种数都出现最多，一定有合法构造方案）。

设严格众数为 $1$，其它数为 ${B_1,B_2,\dots,B_m}$，那么尽量填 $1$，$n-m>\sum_{i=1}^m(P-B_i)+(P-1)$ 则不合法。

只需要用数列总和不为 $P$ 倍数的方案数，减去数列总和不为 $P$ 倍数且 $1$ 出现过多的方案数乘 $P-1$，背包即可。

> 关于严格众数为 $1$ 和为其它的为什么方案数一样多：
>
> 设严格众数为 $x$，给数列所有数都乘上 $x$ 模 $P$ 意义下的逆元，把严格众数变为 $1$，这样并不会改变一个数或几个数的和是否为 $P$ 的倍数，用新数列来求即可。

#### AGC008F Black Radius（表示方法唯一）

https://www.luogu.com.cn/article/pzjwoci5

去掉每个点都染黑的情况，发现每种染色情况可以被一个或多个 $(x,d)$ 表示（不存在 $x$、$y$ 相邻且 $(x,d)$ 和 $(y,d)$ 相等），如果一种染色情况只在最小的 $d$ 计算贡献，那么就不会算重。

对换根法还不是很熟，学到了如何用父亲除儿子外的答案更新儿子的答案（而不是通过记录儿子前缀、后缀的答案）。

#### AGC043D Merge Triplets

对于块 $(a,b,c)$，若 $a>b$，可分为 $(a) (b,c)$；同理若 $b>c$，可分为 $(b)(c)$。

最后得到的块内都是递减的，答案就是这样划分的数量（注意长度 $2$ 的块的数量 $\leq$ 长度 $1$ 的块的数量）。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=2005;
int n,M,P=3000;
int f[N*3][N*6];//前 i 个数，长度 1 的块比 2 的块多的个数（最终需\geq 0) 
void Add(int &x,int y) {
	x+=y;
	if(x>=M) x-=M;
}
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>n>>M;
	n*=3;
	f[0][P]=1;
	for(int i=1;i<=n;i++) {
		for(int j=P-i/2;j<=P+i;j++) {
			f[i][j]=f[i-1][j-1];
			if(i>=2) Add(f[i][j],1ll*f[i-2][j+1]*(i-1)%M);//i 已经固定为新块最大值，第二个数可以为 i-1，或者 i-1 把任意一个数置换出来
			if(i>=3) Add(f[i][j],1ll*f[i-3][j]*(i-1)%M*(i-2)%M);// 同理 
		}
	}
	int ans=0;
	for(int i=P;i<=P+n;i++)
		Add(ans,f[n][i]);
	cout<<ans;
	return 0;
}
```

#### AGC012F Prefix Median

https://www.luogu.com.cn/article/zfm860x6

设 $a$ 为一个 $1\sim 2n-1$ 的排列。

必要条件：

1. $b_i\in[i,2n-i]$，因为 $b_i$ 要保证有 $i-1$ 个大于、小于它的数。
2. 不存在 $i<j$ 满足 $\min(b_j,b_{j+1})<b_i<\max(b_j,b_{j+1})$。

用官方题解的证法证明其充分性。

从后往前考虑，发现候选值的区间 $[i,2n-i]$ 扩展，且 $b_i$、$b_{i-1}$ 之间的值不能作为候选项，考虑 DP（其实我根本没想到，看的题解）。

设 $f_{i,j,k}$ 表示已经填了 $b_{i\sim n}$，$<b_i$ 有 $j$ 个候选项，$>b_i$ 有 $k$ 个候选项的方案数，转移即可。

```cpp {21-23}
#include<bits/stdc++.h>
using namespace std;
const int N=55,M=1e9+7; 
int n,a[N*2];
int f[N][N*2][N*2];
void Add(int &x,int y) {
	x+=y;
	if(x>=M) x-=M;
}
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>n;
	for(int i=1;i<=n*2-1;i++) cin>>a[i];
	sort(a+1,a+n*2);
	f[n][0][0]=1;
	for(int i=n;i>1;i--)  {
		int x=(a[i]!=a[i-1]),y=(a[n*2-i]!=a[n*2-(i-1)]);
		for(int j=0;j<=n*2;j++) {
			for(int k=0;k<=n*2;k++) {
				if(!f[i][j][k]) continue; 
				Add(f[i-1][j+x][k+y],f[i][j][k]);
				for(int _j=1;_j<=j+x;_j++) Add(f[i-1][_j-1][k+1+y],f[i][j][k]);//向前转移的时候，中位数左移，<b_i 的候选项减少 b_{i-1} 和 b_i 之间的数量，>b_i 的候选项增加 1（b_i成为候选项）+y 个 
				for(int _k=1;_k<=k+y;_k++) Add(f[i-1][j+1+x][_k-1],f[i][j][k]);
			}
		}
	}
	int ans=0;
	for(int j=0;j<=n*2;j++)
		for(int k=0;k<=n*2;k++)
			Add(ans,f[1][j][k]);
	cout<<ans;
	return 0;
}
```

#### P10764 [BalticOI 2024] Wall

题目很明确是要求所有情况的 $\sum(H_i-h_i)$，其中 $H_i=\min(\max_{j=1}^ia_j,\max_{j=i}^na_j)$。

拆成 $\sum H_i$ 和 $\sum h_i=2^{n-1}\sum(a_i+b_i)$。

套路地，$\sum H_i=\sum_{x=1}^{\infty}[H_i\ge x]$，也就是枚举 $x$，计算每个位置 $i$ 的 $H_i\ge x$ 的方案数之和。

对 $x$ 作扫描线，$\ge x$ 的看作 $1$，$<x$ 的看作 $0$，那么 $H_i\ge x$ 也就是 $[1,i]$、$[i,n]$ 分别至少有一个 $1$。

对于每个位置 $i$，取值可能有 $0/1/2$ 个 $1$。

分类讨论：

1. 若存在位置有两个 $1$，设左端点 $L$，右端点 $R$。

    1. 位置 $i\in[L,R]$，满足 $H_i\ge x$，贡献为 $2^n$。
    2. 位置 $i\in[1,L-1]$，$[1,i]$ 至少要有一个 $1$，设 $i$ 及左侧有 $k$ 个位置有一个 $1$，贡献 $(2^k-1)2^{n-k}=2^n-2^{n-k}$，$2^n$ 的固定贡献提出去，$2^{n-k}$ 因为每次把 $0$ 改为 $1$ 它及它右侧的位置会乘上 $\frac{1}{2}$，所以用线段树维护。
    3. 位置 $i\in[R+1,n]$ 同上。

2. 否则，不存在位置有两个 $1$，设位置 $i$ 及左侧 $c$ 个位置有一个 $1$，$i$ 及右侧 $d$ 个位置有一个 $1$，总共 $all$ 个位置有一个 $1$。

    1. 若位置 $i$ 有一个 $1$，贡献 $(2^{c-1}-1)(2^{all-c}-1)2^{n-all}+2^{n-1}=2^n-2^{n-c}-2^{n-all+c-1}+2^{n-all}$。
    2. 若位置 $i$ 没有 $1$，贡献 $(2^c-1)(2^{all-c}-1)2^{n-all}=2^n-2^{n-c}-2^{n-all+c}+2^{n-all}$。

    $2^n+2^{n-all}$ 对每个 $i$ 都相同，$2^{n-c}$ 和 $2^{n-d}$ 用线段树维护。

```cpp
#include<bits/stdc++.h>
#define ls k<<1
#define rs k<<1|1
using namespace std;
const int N=5e5+5,MOD=1e9+7,INV2=(MOD+1)/2;
int n,a[N],b[N],c[N*2],tot;
int pw2[N],ucnt[N];
int sum,ans;
struct SEG {
	int S[N*4],Lz[N*4];
	void PushDown(int k) {
		S[ls]=1ll*S[ls]*Lz[k]%MOD;
		S[rs]=1ll*S[rs]*Lz[k]%MOD;
		Lz[ls]=1ll*Lz[ls]*Lz[k]%MOD;
		Lz[rs]=1ll*Lz[rs]*Lz[k]%MOD;
		Lz[k]=1;
	}
	void PushUp(int k) {
		S[k]=(S[ls]+S[rs])%MOD;
	}
	void Build(int k,int l,int r,int v) {
		Lz[k]=1;
		if(l==r) {
			S[k]=v;
			return;
		}
		int mid=l+r>>1;
		Build(ls,l,mid,v);
		Build(rs,mid+1,r,v);
		PushUp(k);
	}
	void Modify(int k,int l,int r,int x,int y,int v) {
		if(x>r||y<l) return;
		if(x<=l&&r<=y) {
			S[k]=1ll*S[k]*v%MOD;
			Lz[k]=1ll*Lz[k]*v%MOD;
			return;
		}
		int mid=l+r>>1;
		PushDown(k);
		if(x<=mid) Modify(ls,l,mid,x,y,v);
		if(y>mid) Modify(rs,mid+1,r,x,y,v);
		PushUp(k);
	}
	int Query(int k,int l,int r,int x,int y) {
		if(x>r||y<l) return 0;
		if(x<=l&&r<=y) return S[k];
		int mid=l+r>>1,ans=0;
		PushDown(k);
		if(x<=mid) ans=Query(ls,l,mid,x,y);
		if(y>mid) ans=(ans+Query(rs,mid+1,r,x,y))%MOD;
		return ans;
	}
}T1,T2;
vector<int> pos[N*2];
void Add(int &x,int y) {
	x+=y;
	if(x>=MOD) x-=MOD;
}
void Init() {
	pw2[0]=1;
	for(int i=1;i<=n;i++) pw2[i]=2*pw2[i-1]%MOD;
}
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>n;
	
	Init();
	
	for(int i=1;i<=n;i++) cin>>a[i],c[++tot]=a[i],Add(sum,a[i]);
	for(int i=1;i<=n;i++) cin>>b[i],c[++tot]=b[i],Add(sum,b[i]);
	Add(ans,MOD-1ll*sum*pw2[n-1]%MOD);
	sort(c+1,c+tot+1);
	tot=unique(c+1,c+tot+1)-c-1;
	for(int i=1;i<=n;i++) {
		a[i]=lower_bound(c+1,c+tot+1,a[i])-c;
		b[i]=lower_bound(c+1,c+tot+1,b[i])-c;
		pos[a[i]].push_back(i);
		pos[b[i]].push_back(i);
	}
	T1.Build(1,1,n,pw2[n]);
	T2.Build(1,1,n,pw2[n]);
	int all=0,L=0,R=0;
	for(int i=tot;i>0;i--) {
		for(int x:pos[i]) {
			++ucnt[x];
			if(ucnt[x]==2) {
				--all;
				if(!L) {
					L=R=x;
					T1.Build(1,1,n,pw2[n]);
					T2.Build(1,1,n,pw2[n]);
					for(int j=1;j<L;j++)
						if(ucnt[j]==1) T1.Modify(1,1,n,j,n,INV2);
					for(int j=R+1;j<=n;j++)
						if(ucnt[j]==1) T2.Modify(1,1,n,1,j,INV2);
				} else {
					L=min(L,x);
					R=max(R,x);
				}
			} else {
				++all;
				if(!L) {
					T1.Modify(1,1,n,x,n,INV2);
					T2.Modify(1,1,n,1,x,INV2);
				} else {
					T1.Modify(1,1,n,x,n,INV2);
					T2.Modify(1,1,n,1,x,INV2);
				}
			}
		}
		int tmp=0;
		if(!L) {
			Add(tmp,1ll*(pw2[n]+pw2[n-all])*n%MOD);
			Add(tmp,MOD-T1.S[1]);
			Add(tmp,MOD-T2.S[1]);
		} else {
			Add(tmp,1ll*pw2[n]*n%MOD);
			Add(tmp,MOD-T1.Query(1,1,n,1,L-1));
			Add(tmp,MOD-T2.Query(1,1,n,R+1,n));
		}
		Add(ans,1ll*tmp*(c[i]-c[i-1])%MOD);
	}
	cout<<ans;
	return 0;
}
```

#### 鸣谢

1. [Slide] 组合计数问题的常用技巧-彭博-2021
2. 某场 CSP 模拟赛