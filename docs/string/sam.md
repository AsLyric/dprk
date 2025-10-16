---
title: 从后缀树到后缀自动机
published: 2025-08-28
updated: 2025-08-28
tags: [字符串, KMP]
category: 信息学
draft: false
---


> 死亡警告：后缀自动机可能会让您放弃学习字符串数据结构，引理、概念、应用什么的真是太抽象了欸。
>
> 写得不清楚不要喷，可以去看别的博客。

# 后缀 Trie

因为从前往后插入字符，每个后缀都会改变，因此我们从后往前插入字符，每次只会新增一个后缀。

维护 $front_{u,c}$ 表示 $u$ 节点代表的后缀，在**前面**添加一个 $c$，对应的新节点（如果有）。

构造步骤（$S$ 在前面插入字符 $c$ 时）：

1. **向上查找**：从代表当前 $S$ 的点向上遍历，直到找到第一个存在 $front_c$ 的节点 $u$。
2. **构建新链**：从 $front_{u,c}$ 开始，向下新建一条链，表示了 $c+S$ 的所有前缀。
3. 更新 $front$：将第一步向上走过的所有点的 $front_c$ 指向新链的第一个节点。

# 后缀树

后缀 Trie $O(n^2)$ 的时间和空间复杂度无法接受，我们试着只保留所有儿子多于一个的节点和后缀节点（对应原串某个后缀的节点）。

节点 $u$ 所表示的前缀中长度最小为 $len_{fa_u}+1$，最大为 $len_u$。（事实上节点 $u$ 表示了一些 $\text{endpos}$ 等价的串，也就是 $u$ 表示一些串构成的等价类。）

但节点的压缩可能导致 $front$ 指向的点不在了，此时需拆点。

以下代码来自 zjk，解释来自豆包。

```cpp
int ch[N][26],len[N],fa[N],cnt;
int insert(int las,int c)
{
	int u=++ct;//新建节点 u（对应新后缀）
	len[u]=len[las]+1;
	while(las&&!ch[las][c])ch[las][c]=u,las=fa[las];//向上找 las 的祖先，直到找到有 c 转移的节点 v
	int v=ch[las][c];
	if(!las)fa[u]=1;//找不到，u 的父亲是根
	else if(len[v]==len[las]+1)fa[u]=v;//v 是 las 的直接儿子，无需拆点
	else {
    	int cl=++ct;//将 v 拆分为等价类 cl 和等价类 v
    	len[cl]=len[las]+1;//cl 在 las 上新增了 c，最大长度加 1
        fa[cl]=fa[v];//继承 v 的父亲
    	for(int i=0;i<26;i++)ch[cl][i]=ch[v][i];//复制 v 的所有转移
    	fa[v]=fa[u]=cl;//更新 v 和 u 的父亲为 cl
    	while(las&&ch[las][c]==v)ch[las][c]=cl,las=fa[las];//修改所有指向 v 的转移为 cl
    }
	return u;
}
```

## 复杂度

咕咕

# 后缀自动机

事实上建后缀树可以用后缀数组，[捉到一只十级勾大佬表示不会串串技术](https://www.cnblogs.com/myee/p/when-building-SuffixTree-with-SuffixArray.html)。

但是不要忘记它的名字——$front$！

从前往后用后缀树的构建方法，就可以建出后缀自动机啦。（$next$ 对应 $front$。）

$next$ 还有一些性质，以后再来补。

# 鸣谢

1. [Slide] 字符串-张隽凯-2025
2. [Post] [后缀树-OI Wiki](https://oi-wiki.org/string/suffix-tree/)
3. 豆包为作者理解课件提供的帮助