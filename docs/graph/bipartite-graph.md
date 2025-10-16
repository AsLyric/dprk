# 二分图

没有奇环的无向图一定是二分图。

判定二分图可以靠给一条边的两个端点染不同的颜色，不能出现两个端点颜色相同。

# 最大匹配

设为 $P$。网络流或匈牙利算法求出，以下为匈牙利算法的版本，复杂度 $O(n^3)$。

```cpp
bool dfs(int x){
    for(int i=fir[x];i;i=nex[i]){
        int v=to[i];
        if(!vis[v]){
        	vis[v]=1;
        	if(!match[v]||dfs(match[v])){
	            match[v]=x;
	            match[x]=v;
	            return 1;
	        }
        }
    }
    return 0;
}
int main() {
	//对于每个二分图左部的点进行匹配
	for(int i=1;i<=n;i++) {
		memset(vis,0,sizeof vis);
		if(dfs(i)) ++ans;
	}
}

```

# 完美匹配

包含了所有点的匹配。

# 多重匹配

拆点或者网络流。

# 最大权完美匹配（待补充）

P6577 【模板】二分图最大权完美匹配

# 最小点覆盖（Kőnig定理）

等于 $P$。

# 最大独立集

等于 $|V|-P$。

# DAG 最小不相交路径覆盖

将每个点拆成出点 $x_{out}$ 和入点 $x_{in}$，对于 DAG 边 $(u,v)$，连一条二分图上 $(u_{out},v_{in})$ 的边。

等于 $|V|-P$。

# DAG 最小可相交路径覆盖/最小链划分

最小链划分中每个点恰好属于一条链，且链尽量少。“链”不需要是连续的一条链，一条连续的链去掉中间一些点也可以算“链”。

或者也可以理解最小可相交（可重）路径覆盖，连续的路径，并且允许多条路径重复经过同一个点，路径尽量少。

Floyd 传递闭包后，变成了上面那个**DAG 最小路径不相交覆盖**问题。

对于有向有环图，其实可以先缩点，例题ABC374G Only One Product Name。

# DAG 最大反链

反链：互相不可达的一些点的集合。

Dilworth 定理：对于任意有限偏序集，其最大反链中元素的数目必等于最小链划分中链的数目。

#### P4298 [CTSC2008] 祭祀

https://www.luogu.com.cn/article/jhpww84r

# 补图是二分图，最大团等于补图最大独立集

P2423 [HEOI2012] 朋友圈

# Hall 定理

# 三分图

不要试图使用两次匈牙利算法贪心，被 Hack 了。

P1231 教辅的组成

P1402 酒店之王

P2891 [USACO07OPEN] Dining G

# 鸣谢

1. [Post] [浅谈网络流的各种建模方法-strcmp](https://www.luogu.com.cn/article/k2hh2bok)