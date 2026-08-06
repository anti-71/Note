---
名称: x^n f(x) 的积分极限
节: 高等数学
tags:
  - 知识点
index: 15
---
##### 结论

设 $f(x)$ 在 $[0,1]$ 上导函数连续，则

1. $\displaystyle\lim_{n\to\infty}\int_{0}^{1}x^{n}f(x)\,dx=0$
2. $\displaystyle\lim_{n\to\infty}n\int_{0}^{1}x^{n}f(x)\,dx=f(1)$

> [!success] 证明
> （1）$f(x)$ 连续，故 $|f(x)|\leqslant M$
>
> $$
> 0\leqslant\left|\int_{0}^{1}x^{n}f(x)\,dx\right|
> \leqslant M\int_{0}^{1}x^{n}\,dx=\frac{M}{n+1}\xrightarrow{n\to\infty}0
> $$
>
> 由[[夹逼准则]]得 $\displaystyle\lim_{n\to\infty}\int_{0}^{1}x^{n}f(x)\,dx=0$
>
> （2）
>
> $$
> (n+1)\int_{0}^{1}x^{n}f(x)\,dx
> =\int_{0}^{1}f(x)\,dx^{n+1}
> =f(1)-\int_{0}^{1}x^{n+1}f'(x)\,dx
> \xrightarrow{n\to\infty} f(1)
> $$
>
> 用[[定积分-分部积分法|分部积分]]，且由结论 1 得 $\displaystyle\int_{0}^{1}x^{n+1}f'(x)dx\to0$，故
>
> $$
> \lim_{n\to\infty}n\int_{0}^{1}x^{n}f(x)\,dx
> =\lim_{n\to\infty}\frac{n}{n+1}\cdot(n+1)\int_{0}^{1}x^{n}f(x)\,dx
> =f(1)
> $$

##### 例题

> [!question]
>
> $$
> \lim_{n\to\infty}\int_{0}^{1}\frac{n\cdot x^{n}}{1+e^{x}}\,dx
> $$

令 $f(x)=\dfrac{1}{1+e^{x}}$，由结论 2

$$
\lim_{n\to\infty}n\int_{0}^{1}x^{n}\cdot\frac{1}{1+e^{x}}\,dx
=f(1)=\boxed{\frac{1}{1+e}}
$$
