---
title: 欧拉函数
published: 2025-01-22
updated: 2025-09-29
tags: [数学, 数论]
category: 信息学
draft: false
---

# 欧拉函数

欧拉函数 $\varphi(n)$ 定义为：从 $1$ 到 $n$ 中与 $n$ 互质的数的个数。

若 $n$ 的素因数分解为 $n = p_1^{c_1} p_2^{c_2} \dots p_k^{c_k}$，其中 $p_1, p_2, \dots, p_k$ 为互不相同的素数，则：

$$ \varphi(n) = n \prod_{i=1}^k \frac{p_i-1}{p_i}$$

# 性质

1. $\varphi(1) = 1$。
2. 对于任意质数 $p$，有 $\varphi(p) = p - 1$。
3. 若 $a$ 与 $b$ 互质，则有 $\varphi(ab) = \varphi(a)\varphi(b)$。

欧拉函数是积性函数，但**不是完全积性函数**。

# 计算

由于是积性函数，可以由最上面那个式子，通过线性筛法在 $O(n)$ 时间内计算。

```cpp
phi[1]=1;
for(int i=2;i<=n;i++) {
    if(!f[i]) {
        p[++tot]=i;
        phi[i]=i-1;
    }
    for(int j=1;j<=tot&&i*p[j]<=n;j++) {
        f[i*p[j]]=1;
        if(i%p[j]==0) {
            phi[i*p[j]]=phi[i]*p[j];
            break;
        }
        phi[i*p[j]]=phi[i]*phi[p[j]];
    }
}
```

# 欧拉反演

对于恒等函数 $\text{id}(n) = n$，有：

$$ n = \text{id}(n) = (\varphi \star I)(n) = \sum_{d | n} \varphi(d) $$

# 例题

#### [P2398 GCD SUM](https://www.luogu.com.cn/problem/P2398)