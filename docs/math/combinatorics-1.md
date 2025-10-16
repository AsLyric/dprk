## 组合数

### 组合恒等式（待补充）

可以结合杨辉三角记忆。

1. 斜线：$$\sum_{i=0}^n {k+i \choose i}={k+n+1 \choose n}$$
2. 列：$$\sum_{i=0}^n {n \choose m}={n+1 \choose m+1}$$
3. 行：

	设 $S(n,m)=\sum_{i=0}^m {n \choose m}$，则有：
	$$S(n,m)=S(n,m-1)+{n \choose m}\\S(n,m)=2S(n-1,m)-{n-1 \choose m}$$

#### HDU6333 Problem B. Harvest of Apples

对于每个询问，求：
$$f(n,m)=\sum_{i=0}^m {n \choose i}$$

发现每次左右端点的修改都是 $O(1)$ 的，可以用莫队做。

我写的时候莫队的排序函数有问题，上一次还是[P1903 [国家集训队] 数颜色 / 维护队列](https://www.luogu.com.cn/problem/P1903)。

### Lucas 定理

$p$ 为质数。

$${n \choose k} \equiv {\lfloor \frac{n}{p} \rfloor \choose \lfloor\frac{k}{p} \rfloor}{n \mod p \choose k \mod p}\pmod p$$

### 模 2 意义下的组合数

#### P3773 [CTSC2017] 吉夫特

#### ARC137D Prefix XORs

https://www.luogu.com.cn/article/g3a5nfvw

两个套路拼好题。

考虑 $a_{0,i}$ 对 $a_{m,n}$ 的贡献，即为计算 $(1,i)$ 到 $(m,n)$ 的路径数：${m+n-i-1 \choose n-i}$ 。

然后用 Lucas 定理可以发现 ${a \choose b}$ 为奇数当且仅当二进制下 $b$ 为 $a$ 的子集。（[P3773 [CTSC2017] 吉夫特](https://www.luogu.com.cn/problem/P3773)）

为了方便进行高位前缀和，将序列逆序（$i$ 到 $n-i$），$a_{0,i}$ 的贡献为 ${m+i-1 \choose i} \bmod 2$，为 $1$ 当且仅当 $i$ 为 $m-1$ 补集的子集，然后枚举补集暴力转移可以改为 SOSDP。



#### CF1713F Lost Array

https://www.luogu.com.cn/article/40usr56d

本来在想怎么消元，但看到了比较强的一种做法，考虑 $a$ 是怎么推出来 $b$ 的，先把 $a$ 和副对角线上的数从右到左重新编号为 $0\sim n-1$。

发现 $a_i$ 走到副对角线上的 $k$ 方案数分别是 ${i \choose k}$ ，根据 Lucas 定理可知当且仅当 $i$ 为 $k$ 的超集对 $k$ 有贡献。

副对角线上的 $k$ 走到 $b_j$ 同理，当且仅当 $k$ 为 $j$ 子集。

> 关于子集异或和的逆是子集异或和，证明如下：
>
> 子集反演：$f(S) = \sum \limits_{T \subseteq S} g(T) \Rightarrow g(S) = \sum \limits_{T \subseteq S} (-1)^{|S| - |T|} f(T)$。
>
> 对于异或，由于 $x \oplus x=0$，其实异或的逆运算就是异或，那么有：$f(S) = \oplus _{T \subseteq S} g(T) \Rightarrow g(S) = \oplus_{T \subseteq S}f(T)$，得证。
>
> 超集异或和的逆是超集异或和同理。

我们要根据 $b$ 反推 $a$，先求子集异或和的逆（子集异或和），再求超集异或和的逆（超集异或和）。

SOSDP时，因为我们无法得知 $b_{n\sim all}$ 的值，集合只枚举到 $n-1$。

```cpp {10-11}
#include<bits/stdc++.h>
using namespace std;
const int N=6e5+5;
int n,f[N],K;
int main() {
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	cin>>n;
	K=__lg(n)+1;
	for(int i=0;i<n;i++) cin>>f[i];
	for(int i=0;i<K;i++) for(int j=1;j<n;j++) if((j>>i)&1) f[j]^=f[j^(1<<i)];
	for(int i=0;i<K;i++) for(int j=1;j<n;j++) if((j>>i)&1) f[j^(1<<i)]^=f[j];
	for(int i=n-1;i>=0;i--) cout<<f[i]<<" ";
	return 0;
}
```

本题的启发在于在初始状态和结果状态间寻找中间状态。

## 卡特兰数

（必会）常见公式： 
$$H_n={2n \choose n}-{2n \choose n-1}$$

### 格路计数

这个结合图还是很好理解的，可以去看看别的博客，不过有很多变体。

从 $(0,0)$ 走到 $(n,m)$，不碰到直线 $y=x+b$ 的方案数：

$${n+m \choose n}-{n+m \choose n+b}$$

意义为沿着直线翻折，越过直线的路径对称域从翻折顶点到终点的路径。（翻折到的点以 $y=x+b$ 和 $x=n$ 的交点为横坐标，$y=x+b$ 和 $y=m$ 交点为纵坐标，$(m-b,n+b)$）

#### AGC021E Ball Eat Chameleons

分讨，[兔的题解](https://www.luogu.com.cn/article/xr7hzcao)，[另解](https://www.luogu.com.cn/article/tlczlmkg)。

设红、蓝球的吃的情况为 $(R,B)$。

1. $K<n$ 或 $R<B$，一定无解。
2. $R\geq n+B$，一定有解。
3. $R=B$，那么最后吃的一定是 `B`，转化为第四种情况的 $(R,B-1)$。
4. 否则 $B < R< n+B$，恰有 $R-B$ 只小粉兔多吃了一个 `R`。
    其它 $n-(R-B)$ 只小粉兔红、蓝球吃得一样多，最后吃的一定是 `B`，我们让它们按 `RBRB` 的顺序吃（不符合的 `RB` 对扔给多吃了 `R` 的小粉兔）。
    也就是至少有 $n-(R-B)$ 对 `RB` 的匹配，转化为括号匹配，那么 `RB` 序列每个前缀，`B` 最多比 `R` 多 $B-(n-(R-B))=R-n$ 个（式子意为无需参加匹配的蓝球数量）。
    这是经典的格路计数问题，转化为从 $(0,0)$ 向右向上到 $(B,R)$ ，不碰到直线 $y=x-(R-n)-1$ 的方案数。

## 斯特林数

### 第二类斯特林数

在小球与盒子那道模板题中见到的，**$n \brace m$ 表示将 $n$ 个互相区分的元素，划分为 $m$ 个互不区分的非空子集的方案数**。

（必会）递推式： 
$${n \brace m}=m {n-1 \brace m}+{n-1 \brace m-1}$$

通项公式：$n,m$ 较大时使用，需要二项式反演来证，参 OI Wiki。

$${n \brace m}=\sum\limits_{i=0}^m\dfrac{(-1)^{m-i}i^n}{i!(m-i)!}$$

#### 应用
1. 若要求非空子集互相区分，直接乘上 $m!$ 即可。

## 组合意义

#### P4769 [NOI2018] 冒泡排序

<https://www.luogu.com.cn/article/7chydorq>（略有瑕疵，分讨第二种当前可以填 $mx+1\sim n$，请看评论。）

这题挺难想的，DP转为格路计数的模型，然后用组合式子求解。而有些题是有一些奇葩的限制，或是需要化简，需要用到组合意义的模型，比如下面几道。

#### CF1842G Tenzing and Random Operations

#### AGC001E BBQ Hard

## 二项式定理

内容：
$$(x+y)^n=\sum_{k=0}^n\ C{_n^k} x^k y^{n-k} = \sum_{k=0}^n\ C{_n^k} x^{n-k} y^k$$

### 应用：化简式子

形式 $\sum_{k=0}^n\ C{_n^k} x^k y^{n-k}$ 要想到化为左式。

对于形式 $\sum_{k=0}^n C_n^k x^k$，要想到化为 $\sum_{k=0}^n C_n^k x^k 1^{n-k}=(x+1)^n$。

## 二项式反演

如果满足

$$
f(n) = \sum_{i = 0}^{n} {n \choose i} g(i)
$$

那么有

$$
g(n) = \sum_{i=0}^{n} {n \choose i} (-1)^{n-i} f(i)
$$

如果满足 

$$
f(n)=\sum_{i=n}^m{i \choose n}g(i)
$$

那么有 

$$
g(n)=\sum_{i=n}^m{i \choose n}(-1)^{i-n}f(i)
$$

## 鸣谢

1. [Slide] 组合计数问题的常用技巧-彭博-2021（部分题目）