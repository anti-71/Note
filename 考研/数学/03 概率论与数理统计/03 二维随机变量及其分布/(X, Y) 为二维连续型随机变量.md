---
名称: (X, Y) 为二维连续型随机变量
章: 第三章 二维随机变量及其分布
节: "[[第五节 随机变量函数的分布]]"
tags:
  - 知识点
index: 2
---
已知 $(X,Y)$ 的概率密度为 $f(x,y)$，则 $Z = g(X,Y)$ 可能为离散型，也可能为连续型，甚至既不是连续型也不是离散型

- 若 Z 是离散型，则求 Z 的分布律；
- 若 Z 是连续型，则先求 Z 的分布函数，再求概率密度；
- 若 Z 既不是连续型也不是离散型，则只求 Z 的分布函数

##### 1）分布函数法

设 $Z = g(X,Y)$ 的分布函数为 $F_Z(z)$（请注意，分布函数的自变量不一定要用 Z，也可以是 $x$，$F_Z(x) = P\{Z \leqslant x\}$），则有

$$
F_Z(z) = P\{Z \leqslant z\} = P\{g(X, Y) \leqslant z\} = \iint\limits_{g(x, y) \leqslant z} f(x, y)dxdy,\ -\infty < z < +\infty
$$

当 $Z = g(X,Y)$ 为连续型随机变量时，其概率密度为 $f_Z(z) = F_Z'(z)$

若求 Z 的概率密度

$$
f_Z(z)=\frac{d}{dz}F_Z(z)=\begin{cases}
积出再导\\\\
不积直接导
\end{cases}
$$

下面介绍不积直接导的公式：

$$
\frac{d}{dz}\int^{\beta(z)}_{\alpha(z)}g(z, x)dx =\int^{\beta(z)}_{\alpha(z)}g'_z(z)dx+g(z,\beta(z))\beta'(z)-g(z,\alpha(z))\alpha'(z)
$$

> [!example] 莱布尼兹公式应用示例
> 求 $f_Z(z)$，其中 $F_Z(z)=\displaystyle\int_0^z dx\int_0^{z-x} \frac12(x+y)\mathrm{e}^{-(x+y)}dy$
>
> $$
> \begin{aligned}
> \frac{d}{dz}F_Z(z) &=
> \frac{d}{dz}\int_0^z g(z,x)\,dx,\quad
> g(z,x)=\int_0^{z-x}\frac12(x+y)\mathrm{e}^{-(x+y)}dy \\[4pt]
> &= \int_0^z g'_z(z,x)\,dx + g(z,z) - 0 \\[4pt]
> &= \int_0^z \frac12 z\mathrm{e}^{-z}\,dx + 0 \\[4pt]
> &= \frac12 z^2\mathrm{e}^{-z}
> \end{aligned}
> $$
>
> 其中 $g'_z(z,x)=\dfrac12 z\mathrm{e}^{-z}$，$g(z,z)=0$

> [!note]
> 分布函数法的难点在于，在计算 $F_Z(z)$ 的过程中，经常需要对变量 Z 进行分段讨论
>
> 讨论的分段点为 $Z = g(X,Y)$ 的最小值 $a$ 和最大值 $\beta$，$(X,Y)$ 取 $f(x,y) > 0$ 的 $(x,y)$

##### 2）公式法

1. $U = \max\{X,Y\}$ 的分布函数

$$
\begin{align*}
F_U(x) &= P\{\max\{X, Y\} \leqslant x\} = P\{X \leqslant x, Y \leqslant x\} \ (\text{(X, Y) 落在点 (x, x) 左下方区域的概率}) \\
&\xlongequal{\text{若 X, Y 独立}} P\{X \leqslant x\}P\{Y \leqslant x\} \\
&= F_X(x)F_Y(x) \\
&\xlongequal{\text{若 X, Y 同分布}} F_X^2(x)\end{align*}
$$

2. $V = \min\{X,Y\}$ 的分布函数

$$
 \begin{align*}
 F_V(x) &= P\{\min\{X, Y\} \leqslant x\}= 1 - P\{\min\{X, Y\} > x\}= 1 - P\{X > x, Y > x\} \\
 &\xlongequal{\text{若 $X,Y$ 独立}} 1 - P\{X > x\}P\{Y > x\} \\
 &= 1 - [1 - F_X(x)][1 - F_Y(x)] \\
 &\xlongequal{\text{若 $X,Y$ 同分布}} 1 - [1 - F_X(x)]^2
 \end{align*}
$$

如果随机变量 $X_1,X_2,\cdots,X_n$ 相互独立，$X_i$ 的概率密度为 $f_i(x)$，分布函数为 $F_i(x)$，$i = 1,2,\cdots,n$，记

$$
M = \max\{X_1, X_2,\cdots, X_n\}, N = \min\{X_1, X_2,\cdots, X_n\}
$$

则 M 和 N 的分布函数和概率密度分别为

$$
F_M(x) = P\{M \leqslant x\} = F_1(x)F_2(x)\cdots F_n(x), f_M(x) = F_M'(x);
$$

$$
F_N(x) = P\{N \leqslant x\} = 1 - [1 - F_1(x)][1 - F_2(x)]\cdots [1 - F_n(x)], f_N(x) = F_N'(x)
$$

如果随机变量 $X_1,X_2,\cdots,X_n$ 相互独立同分布，且 $f_i(x) = f(x),F_i(x) = F(x),i = 1,2,\cdots,n$，则

$$
F_M(x) = [F(x)]^n, f_M(x) = n [F(x)]^{n-1}f(x);
$$

$$
F_N(x) = 1 - [1 - F(x)]^n, f_N(x) = n [1 - F(x)]^{n-1}f(x)
$$

3. 设随机变量 $X$ 和 $Y$ 相互独立，且 $X \sim N(\mu_1,\sigma_1^2),Y \sim N(\mu_2,\sigma_2^2)$，则

$$
Z = X + Y \sim N(\mu_1 + \mu_2,\sigma_1^2 + \sigma_2^2)
$$

即独立正态随机变量的线性组合仍为正态随机变量