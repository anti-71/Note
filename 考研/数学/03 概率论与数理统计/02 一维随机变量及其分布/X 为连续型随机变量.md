---
名称: X 为连续型随机变量
章: 第二章 一维随机变量及其分布
节: "[[第五节 随机变量函数的分布求解]]"
tags:
  - 知识点
index: 2
---
若 X 的概率密度为 $f_X(x)$，则 $Y = g(X)$ 可能为连续型，也可能为离散型，甚至既不是连续型也不是离散型

- 若 Y 是 **离散型**，则求 Y 的分布律；
- 若 Y 是 **连续型**，则先求 Y 的分布函数，再求概率密度；
- 若 Y **既不是连续型也不是离散型**，则只求 Y 的分布函数

当 $Y = g(X)$ 为连续型或者既非离散型也非连续型时，用下列 **分布函数法** 求出 Y 的分布函数：

$$
 F_Y(y) = P\{Y \leqslant y\} = P\{g(X) \leqslant y\} = P\{\varphi(y) \leqslant X \leqslant \psi(y)\} = \int_{\varphi(y)}^{\psi(y)} f_X(x) \mathrm{d}x
$$

若 Y 为连续型，则求导可得 Y 的概率密度 $f_Y(y) = F_Y'(y)$

> [!warning] 注意
> 1. $\displaystyle\int_{\varphi(y)}^{\psi(y)} f_X(x) \mathrm{d}x$ 的计算过程中，经常需要对 Y 进行分段讨论
> 2. 处理 $P\{g(X) \leqslant y\}$ 关键是把事件 $\{g(X) \leqslant y\}$ 转化为 X 在某区间内取值的形式，然后转化为积分求 $P\{g(X) \leqslant y\}$