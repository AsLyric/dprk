---
title: 数论入门
published: 2025-01-20
updated: 2025-08-19
tags: [数学, 数论]
category: 信息学
draft: false 
---

会的东西太少了，不过 CSP/NOIP 应该够了。/(ㄒoㄒ)/~~

# 质数

## 欧拉筛

## 线性筛

#### CF1749D Counting Arrays

显然，数组“不模糊”的充要条件是对于每个 $i$，$a_i$ 与 $[2,i]$ 均不互质，也即分解形式下 $[2,i]$ 中的所有质因数至少出现一次（否则存在一种 $b_j\not =1$ 的方案）。

用个筛子，那么这题就做完了。

# 因数倍数

$a,b$ 最大公因数记为 $\gcd(a,b)$，无歧义时可记为 $(a,b)$。

$a,b$ 最小公倍数记为 $\text{lcm}(a,b)$，无歧义时可记为 $[a,b]$。

$a$ 是 $b$ 的倍数 相当于 $b$ 是 $a$ 的因数 相当于 $b$ 能整除 $a$ 相当于 $a$ 能够被 $b$ 整除 相当于 $b|a$

## $\gcd$

往数集中加入一个数，要么 $\gcd$ 不变，要么最多变为原来的 $\frac{1}{2}$。

# 同余方程

## 裴蜀定理

对于任意自然数 $a,b$，存在整数 $x,y$ 满足 $ax+by=\gcd(a,b)$。

## 线性同余方程

形如 $ax \equiv b \pmod m$，转化为求 $ax+my=b$ 的解。

若 $gcd(a,m) \nmid b$ 则无解。

否则利用 exgcd 求得一组 $ax+my=\gcd(a,m)$ 的特解 $x_0,y_0$。

方程的通解为：
$$
\left\{
\begin{aligned}
x=x_0+\frac{km}{\gcd(a,m)}\\
y=y_0-\frac{ka}{\gcd(a,m)}
\end{aligned}
\right.
$$

### exgcd/拓展欧几里得算法

貌似这种写法比较多。

```cpp
int exgcd(int a,int b,int &x,int &y){
	if(!b){x=1,y=0;return a;}
	int d=exgcd(b,a%b,x,y);
	int tmp=x;
	x=y;y=tmp-a/b*y;
	return d;
}
```

将递$x,y$ 交换也可以这样写，注意区别：

```cpp
int exgcd(int a,int b,int &x,int &y){
	if(!b){x=1,y=0;return a;}
	int d=exgcd(b,a%b,y,x);
	y-=a/b*x;
	return d;
}
```

## CRT

NOIP 应该没有这么裸的题吧，EXCRT 什么的就不学了。

对于

$$
\left\{
\begin{aligned}
x \equiv a_1 \pmod {m_1} \\
x \equiv a_2 \pmod {m_2} \\
\text{...} \\
x \equiv a_n \pmod {m_n}
\end{aligned}
\right.
$$

设 $MOD=\prod_{i=1}^n m_i,M_i= \frac{MOD}{m_i}$。

令 $t_i$ 表示 $M_i$ 在模 $m_i$ 意义下的逆元。

则$ans=(\sum_{i=1}^n a_i M_i t_i)\mod MOD$。

## BSGS 求解高次同余方程

求 $a^x$ 模 $m$ 意义下同余于 $b$ 的解。

任何解可以表示为 $p\sqrt m+q$，哈希求解。

#### P3846 [TJOI2007] 可爱的质数/【模板】BSGS

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
ll p,a,b;
ll ksm(ll x,ll y) {
	ll ret=1;
	while(y) {
		if(y&1) ret=ret*x%p;
		x=x*x%p;
		y>>=1;
	}
	return ret;
}
ll BSGS() {
	unordered_map<ll,ll>mp;
	ll t=sqrt(p)+1;
	ll tmp=b;
	for(ll i=0;i<t;i++) {
		mp[tmp]=i;
		tmp=tmp*a%p;
	}
	ll c=ksm(a,t);
	if(!c) return b==0?1:-1;
	tmp=c;
	for(ll i=1;i<=t;i++) {
		if(mp.count(tmp)) {
			ll j=mp[tmp];
			if(i*t-j>=0) return i*t-j;
		}
		tmp=tmp*c%p;
	}
	return -1;
}
int main() {
	scanf("%lld%lld%lld",&p,&a,&b);
	ll ans=BSGS();
	if(ans==-1) puts("no solution");
	else printf("%lld",ans);
	return 0; 
}
```

# 数论分块（整除分块）

### P2261 [CQOI2007] 余数求和