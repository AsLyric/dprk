## 算法

### E-K

复杂度上限 $O(nm^2)$。

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=205,M=5005;
int n,m,s,t;
int fir[N],nex[M*2],to[M*2],val[M*2],tot=1;
int flow[N];
ll ans;
int pre[N];
void add(int x,int y,int v){
	nex[++tot]=fir[x],to[tot]=y,val[tot]=v,fir[x]=tot;
}
bool bfs(){
	queue<int>q;
	for(int i=1;i<=n;i++)
		pre[i]=-1;
	flow[s]=2147483647;
	q.push(s);
	while(!q.empty()){
		int u=q.front();q.pop();
		if(u==t) return 1;
		for(int i=fir[u];i;i=nex[i]){
			int v=to[i],w=val[i];
			if(w>0&&pre[v]==-1){
				pre[v]=i;
				flow[v]=min(flow[u],w);
				q.push(v);
			}
		}
	}
	return 0;
}
int main(){
	scanf("%d%d%d%d",&n,&m,&s,&t);
	for(int i=1,x,y,v;i<=m;i++){
		scanf("%d%d%d",&x,&y,&v);
		add(x,y,v);
		add(y,x,0);
	}
	while(bfs()){
		ans+=flow[t];
		int x=t;
		while(x!=s){
			int i=pre[x];
			val[i]-=flow[t];
			val[i^1]+=flow[t];
			x=to[i^1];
		}
	}
	printf("%lld",ans);
	return 0;
}
```



### Dinic

复杂度上限 $O(n^2m)$，在稠密图上比 E-K 算法有优势，二分图上复杂度会小很多。

使用当前弧优化，避免已经被用完的边再用。

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=205,M=5005;
const ll INF=0x3f3f3f3f3f3f3f3f;
int n,m,s,t;
int fir[N],now[N],nex[M*2],to[M*2],val[M*2],tot=1;
ll ans,d[N];
void add(int x,int y,int v){
	nex[++tot]=fir[x],to[tot]=y,val[tot]=v,fir[x]=tot;
}
bool bfs(){
	queue<int>q;
	q.push(s);
	for(int i=1;i<=n;i++) d[i]=INF;
	d[s]=0;
	now[s]=fir[s];
	while(!q.empty()){
		int u=q.front();q.pop();
		for(int i=fir[u];i;i=nex[i]){
			int v=to[i];
			if(val[i]>0&&d[v]==INF){
				now[v]=fir[v];
				d[v]=d[u]+1;
				if(v==t) return 1;
				q.push(v);
			}
		}
	}
	return 0;
}
ll dfs(int u,ll sum){
	if(u==t) return sum;
	ll tmp,ret=0;
	for(int i=now[u];i&&sum;i=nex[i]){
		now[u]=i;
		int v=to[i];
		if(val[i]>0&&d[v]==d[u]+1){
			tmp=dfs(v,min(sum,(ll)val[i]));
			val[i]-=tmp;
			val[i^1]+=tmp;
			sum-=tmp;
			ret+=tmp;
		}
	}
	return ret;
}
int main(){
	scanf("%d%d%d%d",&n,&m,&s,&t);
	for(int i=1,x,y,v;i<=m;i++){
		scanf("%d%d%d",&x,&y,&v);
		add(x,y,v);
		add(y,x,0);
	}
	while(bfs()){
		ans+=dfs(s,INF);
	}
	printf("%lld",ans);
	return 0;
}
```

### ISAP

出题人不玩原神的话一般不会卡 Dinic，毕竟网络流考察的并不是板子，ISAP 还有 HLPP 等可能在工程中有用？

## 最大流最小割定理

网络**最大流**等于**最小割**。

### 拆点

用于有点的流量限制或多重匹配。

### 划分问题

P4313 文理分科

### 染色

P2774 方格取数问题

P3355 骑士共存问题

### 平面图最大流/最小割

等于对偶图最短路。

P4001 [ICPC-Beijing 2006] 狼抓兔子

P7916 [CSP-S 2021] 交通规划

P2046 [NOI2010] 海拔

## 费用流

P2762 太空飞行计划问题

## 鸣谢

1. [Post] [浅谈网络流的各种建模方法-strcmp](https://www.luogu.com.cn/article/k2hh2bok)