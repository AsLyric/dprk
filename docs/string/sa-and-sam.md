---
title: 后缀数据结构经典问题
published: 2025-08-19
updated: 2025-08-28
tags: [字符串, 自动机]
category: 信息学
draft: false
---

## 最小循环位移/最小表示法

### SA
将串复制一遍转化为后缀排序问题。

### SAM
将串复制一遍在 SAM 上一直往最小可追加的字符上跳。

## 不同子串个数
[LG2408](https://www.luogu.com.cn/problem/P2408)

### SA
显然用总子串数减去每相邻两个之间相同前缀的数量可以轻松求解。

### SAM
根据 SAM 的性质，每条从 $t_0$ 开始的路径恰好与一个子串相对应。

在 SAM 上计数即可。

## 子串出现次数
### SAM
由于后缀链接 $link$ 为当前等价类中最短串去掉首字母后的状态，也就是说如果等价类 $i$ 出现了 $cnt_i$ 次，那么它的所有后缀也出现了 $cnt_i$ 次。

方法为用建出 $link$ 建出 $parent$ 树或按 $len$ 从大到小转移（$link_i$ 的 $len$ 一定小于 $i$ 的 $len$)：

$$ cnt_{link_i}+=cnt_i $$

## 第 $k$ 大子串
[LG3975](https://www.luogu.com.cn/problem/P3975)

若不同位置出现的相同串算一个，与经典问题 1 类似。

否则与经典问题 2 类似。

## 多个字符串间的最长公共子串
[LOJ171](https://loj.ac/p/171) [LG2463](https://www.luogu.com.cn/problem/P2463) [SP1812](https://www.luogu.com.cn/problem/SP1812)

### SA
每个串之间添加不同的分隔符，二分答案并 check 如下：

```cpp
bool Check(int x) {
	while(top) vis[st[top--]]=0;
	for(int i=1;i<=n;i++) {
		if(height[i]<x) {
			while(top) vis[st[top--]]=0;
		}
		if(!vis[col[sa[i]]]) {
			vis[col[sa[i]]]=1;
			st[++top]=col[sa[i]];
			if(top==m) return 1;
		}
	}
	return 0;
}
```

### SAM

每个串之间添加分隔符，为每一个等价类记录一个 $state$ 为包含它的子串集合，在 $parent$ 树上向上传递。（口胡，未验证）