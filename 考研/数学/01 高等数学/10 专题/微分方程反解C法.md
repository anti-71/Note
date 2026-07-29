---
名称: 微分方程反解 C 法
章: 第十章 专题
节: "[[专题四 中值定理的解题方法]]"
tags:
  - 知识点
index: 3
---
##### 思路

将结论中的 $\xi$ 换为 $x$，视为微分方程并求解，将通解中的任意常数 $C$ 反解出来放到等号左边，则左边即为构造的辅助函数 $F(x)$

##### 步骤

1. 结论中的 $\xi\to x$，得到关于 $f(x),f'(x)$ 的方程
2. 解此微分方程，分离变量，得 $F(x)=C$ 的形式
3. 取左侧为 $F(x)$，由罗尔定理或拉格朗日中值定理完成证明

##### 例题 1

> [!question]
>
> 设 $f(x)$ 在 $[a,b]$ 连续，在 $(a,b)$ 可导，$a>0$，$f(a)=0$，求证：$\exists\xi\in(a,b)$ 使 $\displaystyle f(\xi)=\frac{b-\xi}{a}f'(\xi)$

**证明**：将 $\xi$ 换为 $x$，并改写为微分方程

$$
f(x)=\frac{b-x}{a}f'(x)\;\Longrightarrow\; a\,f(x)=(b-x)f'(x)
$$

分离变量：

$$
\frac{f'(x)}{f(x)}=\frac{a}{b-x}
$$

两边积分：

$$
\ln|f(x)|=-a\ln|b-x|+\ln C
\;\Longrightarrow\; f(x)\,(b-x)^{a}=C
$$

取 $F(x)=f(x)\,(b-x)^{a}$，则 $F(a)=f(a)\,(b-a)^{a}=0$，$F(b)=f(b)\cdot0=0$

由[[罗尔定理]]，$\exists\xi\in(a,b)$ 使 $F'(\xi)=0$，即 $\displaystyle f(\xi)=\frac{b-\xi}{a}f'(\xi)$，证毕
