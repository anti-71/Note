---
名称: n 项和极限的拟合法
章: 第十章 专题
节: "[[专题一 数列极限的解题方法]]"
tags:
  - 知识点
index: 1
---
##### 方法

当 n 项和 $\sum f\left(\dfrac{k}{n}\right)\cdot\dfrac{1}{n+g(k)}$ 的分母含变项 $g(k)$ 时，先将分母统一化为 $n$，再证明替换后的差值趋于 0，从而转化为定积分定义求极限

##### 例题

> [!question]
>
> $$
> \lim\limits_{n\to\infty}\left(\dfrac{\sin\dfrac{\pi}{n}}{n+1}+\dfrac{\sin\dfrac{2\pi}{n}}{n+\dfrac{1}{2}}+\dots+\dfrac{\sin\pi}{n+\dfrac{1}{n}}\right)
> $$

将原式分母直接替换为 $n$，证明差值趋于 0

$$
\begin{aligned}
0&\leqslant\Bigg|\sum\limits_{k=1}^n\dfrac{1}{n}\sin\dfrac{k\pi}{n}
-\sum\limits_{k=1}^n\dfrac{1}{n+\dfrac{1}{k}}\sin\dfrac{k\pi}{n}\Bigg|\\
&=\Bigg|\sum\limits_{k=1}^n\left(\sin\dfrac{k\pi}{n}\right)\cdot
\dfrac{\dfrac{1}{k}}{n\left(n+\dfrac{1}{k}\right)}\Bigg|
\leqslant\sum\limits_{k=1}^n\dfrac{1}{n^2}
=\dfrac{n+1}{n^2}=\dfrac{1}{n}\to0
\end{aligned}
$$

故

$$
\lim\limits_{n\to\infty}\sum\limits_{k=1}^n\dfrac{1}{n+\dfrac{1}{k}}\sin\dfrac{k\pi}{n}
=\lim\limits_{n\to\infty}\sum\limits_{k=1}^n\dfrac{1}{n}\sin\dfrac{k\pi}{n}
=\displaystyle\int_0^1\sin\pi xdx=\boxed{\dfrac{2}{\pi}}
$$
