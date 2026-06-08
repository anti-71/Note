---
名称: 指数分布 X ∼ E(λ)
章: 第二章 一维随机变量及其分布
节: "[[第四节 连续型随机变量的概率密度与常见分布]]"
tags:
  - 知识点
index: 3
---
##### 概率密度

$$
 f(x)= \begin{cases} \lambda \mathrm{e}^{-\lambda x},& x > 0\\\\ 0,& x \leqslant 0 \end{cases}
$$

参数 $\lambda > 0$

##### 分布函数

$$
F(x)= \begin{cases} 0,& x < 0\\\\ 1 - \mathrm{e}^{-\lambda x},& x \geqslant 0\end{cases}
$$

> [!note] 指数分布具有 “**无记忆性**”
> 即对任意的 $s > 0，t > 0$，有
> $$
> P\{X > s + t \mid X > s\} = P\{X > t\}
> $$