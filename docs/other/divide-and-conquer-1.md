---
title: CDQ 分治与整体二分
published: 2024-08-19
updated: 2025-08-19
tags: [分治, 二分, 离线]
category: 信息学
draft: false 
---

# CDQ 分治

## 基本思想
面对一个离线的三维偏序问题，用 sort 优化掉一维，分治中优化掉另一维，数据结构优化一维。

## 例题

#### P3810 【模板】三维偏序（陌上花开）

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+5;
int n,k,tot;
int s[N*2];
int ans[N];
struct nd{
    int a,b,c,cnt,sum;
    friend bool operator ==(nd x,nd y){
        return x.a==y.a&&x.b==y.b&&x.c==y.c;
    }
}t[N],q[N];
bool cmp(nd x,nd y){return x.a==y.a?(x.b==y.b?x.c<y.c:x.b<y.b):x.a<y.a;}
bool cmp2(nd x,nd y){return x.b==y.b?x.c<y.c:x.b<y.b;}
void update(int x,int v){for(;x<=2e5;x+=x&(-x))s[x]+=v;}
int ask(int x){int ret=0;for(;x;x-=x&(-x))ret+=s[x];return ret;}
void solve(int l,int r){
    if(l==r)return;
    int mid=l+r>>1;
    solve(l,mid);
    solve(mid+1,r);
    sort(q+l,q+mid+1,cmp2);
    sort(q+mid+1,q+r+1,cmp2);
    int i=mid+1,j=l;
    for(;i<=r;i++){
        while(j<=mid&&q[j].b<=q[i].b){
            update(q[j].c,q[j].cnt);
            ++j;
        }
        q[i].sum+=ask(q[i].c);
    }
    for(int k=l;k<j;k++)
        update(q[k].c,-q[k].cnt);
}
int main(){
    scanf("%d%d",&n,&k);
    for(int i=1;i<=n;i++){
        scanf("%d%d%d",&t[i].a,&t[i].b,&t[i].c);
    }
    sort(t+1,t+n+1,cmp);
    for(int i=1;i<=n;){
        int j=i+1;
        while(j<=n&&t[j]==t[i])++j;
        q[++tot]={t[i].a,t[i].b,t[i].c,j-i};
        i=j;
    }
    solve(1,tot);
    for(int i=1;i<=tot;i++)
        ans[q[i].sum+q[i].cnt-1]+=q[i].cnt;
    for(int i=0;i<n;i++)
        printf("%d\n",ans[i]);
    return 0;
}
```

#### P4690 [Ynoi Easy Round 2016] 镜中的昆虫

把问题转化为更新 $pre$ （前面第一个同色的位置）和求 $\sum_{i=l}^r[pre_i<l]$。

发现修改一个相同颜色段的颜色，只有左端点，和后面第一个同色的段的左端点的 $pre$ 会改变。每次推平操作最多增加 $3$ 个颜色段，删掉多个颜色段，所以插入和删除次数都是 $O(n+m)$ 级别。

显然朴素枚举区间会超时，我们将其转化为三维偏序问题（含时间这一维）。

# 整体二分

## 基本思想

原问题的答案能二分，离线下来整体进行二分。

## 特殊应用

区间第 k 大可以且推荐用主席树求，但矩形中的第 k 大只能使用整体二分求。

## 例题

#### P1527 [国家集训队] 矩阵乘法

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=250005,Q=6e4+5;
const int INF=1e9;
int n,qu,tot;
int ans[Q];
struct nd{
    int op,a,b,c,d,v;
}q[N+Q],lq[N+Q],rq[N+Q];
struct BIT{
    int s[505][505];
	void update(int x,int y,int v){
	    for(int i=x;i<=n;i+=i&(-i))
            for(int j=y;j<=n;j+=j&(-j))
	            s[i][j]+=v;
	}
	int ask(int x,int y){
	    int ret=0;
	    for(int i=x;i;i-=i&(-i))
	        for(int j=y;j;j-=j&(-j))
                ret+=s[i][j];
	    return ret;
	}
    int sum(int a,int b,int c,int d){
        return ask(c,d)-ask(a-1,d)-ask(c,b-1)+ask(a-1,b-1);
    }
}B;
void solve(int l,int r,int st,int ed){//l-r 为区间，st-ed 为询问
    if(ed<st) return;
    if(l==r){
        for(int i=st;i<=ed;i++)
            ans[q[i].op]=l;
        return;
    }
    int mid=l+r>>1;
    int lt=0,rt=0;
    for(int i=st;i<=ed;i++){
        if(q[i].op>0){
            int t=B.sum(q[i].a,q[i].b,q[i].c,q[i].d);
            if(t>=q[i].v) lq[++lt]=q[i];
            else rq[++rt]=q[i],rq[rt].v-=t;
        }else{
            if(q[i].v<=mid) B.update(q[i].a,q[i].b,1),lq[++lt]=q[i];
            else rq[++rt]=q[i];
        }
    }
    for(int i=st;i<=ed;i++)
    	if(q[i].op==0&&q[i].v<=mid) B.update(q[i].a,q[i].b,-1);
    for(int i=1;i<=lt;i++)
        q[st+i-1]=lq[i];
    for(int i=1;i<=rt;i++)
        q[st+lt+i-1]=rq[i];
    solve(l,mid,st,st+lt-1);
    solve(mid+1,r,st+lt,ed);
}
int main(){
    scanf("%d%d",&n,&qu);
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            q[++tot]={0,i,j};
            scanf("%d",&q[tot].v);
        }
	}
    for(int i=1;i<=qu;i++){
        q[++tot].op=i;
        scanf("%d%d%d%d%d",&q[tot].a,&q[tot].b,&q[tot].c,&q[tot].d,&q[tot].v);
    }
    solve(0,INF,1,tot);
    for(int i=1;i<=qu;i++)
        printf("%d\n",ans[i]);
    return 0;
}
```

#### GYM103687D / QOJ3998 The Profiteer

记 $p_i$ 为最小的满足 $[i,p_i]$ 合法的数（$i\le p_i$）。

暴力 $O(n^2k)$。

每个 $p_i$ 可以暴力二分，这样复杂度 $O(n^2k\log n)$，但我们可以选择整体二分，`Solve(vl,vr,ql,qr)` 时，已将不在 $[vl,vr]\cup[ql,qr]$ 的加入背包。

二分 $m=\lfloor\frac{l+r}{2}\rfloor$ 的 $p_m$，二分左端点 $L$ 右移到 $mid+1$，那么我们用提高价格后的 $[L,mid]$ 更新 $f$，右端点 $R$ 左移到 $mid-1$，我们用没有提高价格的更新 $f$，可以优化掉一个 $\log$。

细节比较有意思。

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=2e5+5;
int n,K,p[N];
ll E;
int v[N],a[N],b[N];
vector<int> f;
stack<vector<int> > sta;
void Add(int x,int typ) {
	int w=typ?b[x]:a[x];
	for(int i=K;i>=w;i--) f[i]=max(f[i],f[i-w]+v[x]);
}
bool Check(int l,int r,int x) {
	sta.emplace(f);
	for(int i=l;i<=x;i++) Add(i,1);
	for(int i=x+1;i<=r;i++) Add(i,0);
	ll tmp=0;
	for(int i=1;i<=K;i++) tmp+=f[i];
	f=sta.top();
	sta.pop();
	return tmp<=E*K;
}
void Solve(int vl,int vr,int ql,int qr) {
	if(ql>qr||vl==vr) {
		for(int i=ql;i<=qr;i++) p[i]=vl;
		return;
	}
	sta.emplace(f);
	int mid=ql+qr>>1;
	int L=max(vl,mid),R=vr,ans=n+1;
	for(int i=ql;i<mid;i++) Add(i,0);
	for(int i=mid;i<L&&i<=qr;i++) Add(i,1);
	while(L<=R) {
		int MID=L+R>>1;
		if(Check(L,R,MID)) {
			for(int i=MID;i<=R;i++) Add(i,0);
			ans=MID,R=MID-1;
		} else {
			for(int i=L;i<=MID;i++) Add(i,1);
			L=MID+1;
		}
	}
	p[mid]=ans;
	f=sta.top();
	for(int i=mid;i<=qr&&i<=vl-1;i++) Add(i,1);
	for(int i=ans+1;i<=vr;i++) Add(i,0);
	Solve(vl,ans,ql,mid-1);
	f=sta.top();
	for(int i=ql;i<=mid;i++) Add(i,0);
	for(int i=max(qr+1,vl);i<ans;i++) Add(i,1);
	Solve(ans,vr,mid+1,qr);
	sta.pop();
}
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>n>>K>>E;
	for(int i=0;i<=K;i++) f.emplace_back(0);
	for(int i=1;i<=n;i++) {
		cin>>v[i]>>a[i]>>b[i];
	}
	Solve(1,n+1,1,n);
	ll tmp=0;
	for(int i=1;i<=n;i++) tmp+=n-p[i]+1;
	cout<<tmp;
	return 0;
}
```