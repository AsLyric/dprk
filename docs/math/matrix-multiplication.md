### P2044 [NOI2012] 随机数生成器

绿题不多说了，因为用`long long`会溢出，我用的`__int128_t`，其实可以学习一下龟速乘（用 $y$ 的二进制下的每一个 $1$ 的位权和 $x$ 相乘，那么从低到高，位权乘 $2$ 可以转化为 $x$ 乘 $2$，求和 $y$ 二进制下为 $1$ 的位置的 $x$ 即可）。

### P1397 [NOI2013] 矩阵游戏

[非常帅的十进制快速幂带给大家。](https://www.luogu.com.cn/article/nipre18z)

[另有特殊性质做法，事实上不太好观察到。](https://www.luogu.com.cn/article/xxac6hu3)

另有数论做法：

#### Part1

题中需要快速计算形如 $A=\begin{bmatrix}
a & 0\\
b & 1
\end{bmatrix}$ 的矩阵的幂。

注意到对于对角矩阵的幂，每个主对角线上的数为原数的幂，所以满足欧拉定理的推论。

考虑将 $A$ 对角化，即找到一个可逆矩阵 $P$，满足 $B=PAP^{-1}$ 为对角矩阵，我们称 $A$ 与 $B$ 相似。

这里补一个相似矩阵的知识，矩阵 $A$ 与 $B$ 相似，那么 $A^x=P^{-1}B^xP$。

所以证明 $A^x=A^{x \bmod \varphi(p)}$，只需证 $B^x=B^{x \bmod \varphi(p)}$，易得。

#### Part2

我们需要找到一个 $P$ 使得 $B=PAP^{-1}=\begin{bmatrix}
\lambda_1 & 0\\
0 & \lambda_2
\end{bmatrix}$，也即 $PA=\begin{bmatrix}
\lambda_1 & 0\\
0 & \lambda_2
\end{bmatrix}P$。

设 $P=\begin{bmatrix}
x & y\\
z & w
\end{bmatrix}$，则 $\begin{bmatrix}
ax+by & y\\
az+bw & w
\end{bmatrix}=\begin{bmatrix}
\lambda_1x & \lambda_1y\\
\lambda_2z & \lambda_2w
\end{bmatrix}$。

解得：
$$
\left\{\begin{matrix}
(a-1)x+by=0\\
(a-1)z+bw=0
\end{matrix}\right.
$$
因为 $b\not = 0$，$a=1$ 的情况无解，需要特判，发现就是每次加 $b$，$A^x=A^{x \bmod p}$。

ref：https://www.luogu.com.cn/article/rwbv7g9d