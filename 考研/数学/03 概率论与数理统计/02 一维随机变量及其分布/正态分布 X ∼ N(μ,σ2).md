---
名称: 正态分布 X ∼ N(μ,σ2)
章: 第二章 一维随机变量及其分布
节: "[[第四节 连续型随机变量的概率密度与常见分布]]"
tags:
  - 知识点
index: 4
---
##### 密度函数

$$
f(x) = \frac{1}{\sqrt{2\pi}\sigma} \mathrm{e}^{-\dfrac{(x - \mu)^2}{2\sigma^2}},\ -\infty < x < +\infty
$$

$\mu,\sigma\ (\sigma > 0)$ 为常数

##### 分布函数

$$
F(x) = \int_{-\infty}^{x} \frac{1}{\sqrt{2\pi}\sigma} \mathrm{e}^{-\dfrac{(t - \mu)^2}{2\sigma^2}} \mathrm{d}t,\ -\infty < x < +\infty
$$

> [!note] 凑正态法
> $$
> \int_{-\infty}^{+\infty} \frac{1}{\sqrt{2\pi}\sigma} \mathrm{e}^{-\dfrac{(t - \mu)^2}{2\sigma^2}} \mathrm{d}t = 1
> $$
> 适用于 $e$ 的高次幂积分

##### 性质

$f(x)$ 关于 $x = \mu$ 对称；$F(x)$ 为 $x$ 的单调增加函数

若 $\mu = 0，\sigma = 1$，即 $X \sim N(0,1)$，称 X 服从 **标准正态分布**，其概率密度和分布函数分别记为

$$
\varphi(x) = \frac{1}{\sqrt{2\pi}} \mathrm{e}^{-\dfrac{x^2}{2}},\ \Phi(x) = \int_{-\infty}^{x} \frac{1}{\sqrt{2\pi}} \mathrm{e}^{-\dfrac{t^2}{2}} \mathrm{d}t (-\infty < x < +\infty)
$$

其中 $\Phi(x)$ 的值可通过查标准正态分布表求得，并且有 $\Phi(-x) = 1 - \Phi(x)$

> [!note]
> $$
> \Phi(0)=\frac{1}{2},\ \Phi(1)= 0.8413,\ \Phi(1.645)= 0.95,\ \Phi(1.96)= 0.975
> $$

若 $X \sim N(\mu,\sigma^2)$，则 $\dfrac{X - \mu}{\sigma} \sim N(0,1)$（X 的标准化），从而有 $P\{a < X \leqslant b\} = \Phi\left(\dfrac{b - \mu}{\sigma}\right) - \Phi\left(\dfrac{a - \mu}{\sigma}\right)$

> [!note]
> 以上为万能法
>
> - **特殊法**：看面积（对称性），但并非万能