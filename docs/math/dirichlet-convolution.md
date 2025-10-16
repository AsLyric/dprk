## 定义

狄利克雷卷积是一种重要的数论卷积，其定义为：

$$ t(n) = \sum_{i | n} f(i) g\left( \frac{n}{i} \right) $$

在数论中，$f$ 和 $g$ 的卷积记作 $f \star g$。

## 性质

交换律：$f\star g=g\star f$。

结合律：$(f\star g)\star h=f\star(g\star h)$。

分配律：$f\star(g+h)=f\star g+f\star h$。

若 $f,g$ 积性，则 $h=f\star g$ 积性。

## 常见数论函数的狄利克雷卷积

| $\star$     | $\epsilon$ | $I$  | $d$  | $\mu$      | $\varphi$                   | $\sigma$                            | $\text{id}$            |
| ----------- | ---------- | ---- | ---- | ---------- | --------------------------- | ----------------------------------- | ---------------------- |
| $\epsilon$  | $\epsilon$ | $I$  | $d$  | $\mu$      | $\varphi$                   | $\sigma$                            | $\text{id}$            |
| $I$         |            | $d$  | -    | $\epsilon$ | $\text{id}$                 | $d \star \text{id}$                 | $\sigma$               |
| $d$         |            |      | -    | $I$        | $\sigma$                    | -                                   | $I \star \sigma$       |
| $\mu$       |            |      |      | -          | -                           | $\text{id}$                         | $\varphi$              |
| $\varphi$   |            |      |      |            | $\text{id} \star \text{id}$ | -                                   | -                      |
| $\sigma$    |            |      |      |            |                             | $d \star \text{id} \star \text{id}$ | -                      |
| $\text{id}$ |            |      |      |            |                             |                                     | $\varphi \star \sigma$ |