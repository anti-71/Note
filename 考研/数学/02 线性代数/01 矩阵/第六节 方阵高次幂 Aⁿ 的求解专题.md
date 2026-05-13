---
名称: 第六节 方阵高次幂 Aⁿ 的求解专题
章: 第一章 矩阵
tags:
  - 章节
index: 6
---
### 一、方法总结

- #### [[方阵高次幂 Aⁿ 的求解]]

### 例题

#### 例 1

> [!question] 
> 已知 $A = \begin{pmatrix} 2 & 0 & 2 \\ 0 & 3 & 0 \\ 2 & 0 & 2 \end{pmatrix}$，则 $A^n$（$n$ 为正整数）= $\underline{\qquad}$

因为

$$
 A^2 = \begin{pmatrix} 2 & 0 & 2 \\ 0 & 3 & 0 \\ 2 & 0 & 2 \end{pmatrix} \begin{pmatrix} 2 & 0 & 2 \\ 0 & 3 & 0 \\ 2 & 0 & 2 \end{pmatrix} = \begin{pmatrix} 2^3 & 0 & 2^3 \\ 0 & 3^2 & 0 \\ 2^3 & 0 & 2^3 \end{pmatrix}
$$

$$
A^3 = A^2 A = \begin{pmatrix} 2^3 & 0 & 2^3 \\ 0 & 3^2 & 0 \\ 2^3 & 0 & 2^3 \end{pmatrix} \begin{pmatrix} 2 & 0 & 2 \\ 0 & 3 & 0 \\ 2 & 0 & 2 \end{pmatrix} = \begin{pmatrix} 2^5 & 0 & 2^5 \\ 0 & 3^3 & 0 \\ 2^5 & 0 & 2^5 \end{pmatrix}
$$

观察规律，得

$$
 A^n = \begin{pmatrix} 2^{2n-1} & 0 & 2^{2n-1} \\ 0 & 3^n & 0 \\ 2^{2n-1} & 0 & 2^{2n-1} \end{pmatrix} 
$$

#### 例 2

> [!question] 
> 已知 $A = \begin{pmatrix} 1 & a & b \\ 0 & 1 & a \\ 0 & 0 & 1 \end{pmatrix}$，求 $A^n$（$n$ 为大于 3 的正整数）

由于 $A = \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{pmatrix} + \begin{pmatrix} 0 & a & b \\ 0 & 0 & a \\ 0 & 0 & 0 \end{pmatrix} = E + B$， 而 $B^2 = \begin{pmatrix} 0 & 0 & a^2 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$，$B^3 = O$，故

$$
\begin{align*} A^n &= (E + B)^n \\
&= C_n^0 E^n + C_n^1 E^{n-1}B + C_n^2 E^{n-2}B^2 \\ &= E + nB + \frac{n(n-1)}{2}B^2 \\\\ &= \begin{pmatrix} 1 & na & \dfrac{n(n-1)}{2}a^2 + nb \\ 0 & 1 & na \\ 0 & 0 & 1 \end{pmatrix} \end{align*} 
$$

#### 例 3

> [!question] 
> 已知矩阵 $P = \begin{pmatrix} -1 & -4 \\ 1 & 1 \end{pmatrix}$，$\Lambda = \begin{pmatrix} -1 & 0 \\ 0 & 2 \end{pmatrix}$，且有 $P^{-1}AP = \Lambda$，求 $A^{11}$

由于 $P^{-1}AP = \Lambda$，即 $A = P\Lambda P^{-1}$，故

$$
\begin{align*} A^{11} &= P\Lambda^{11}P^{-1} \\ &= \begin{pmatrix} -1 & -4 \\ 1 & 1 \end{pmatrix} \begin{pmatrix} -1 & 0 \\ 0 & 2^{11} \end{pmatrix} \begin{pmatrix} \dfrac{1}{3} & \dfrac{4}{3} \\ -\dfrac{1}{3} & -\dfrac{1}{3} \end{pmatrix} \\ &= \begin{pmatrix} 1 & -2^{13} \\ -1 & 2^{11} \end{pmatrix} \begin{pmatrix} \dfrac{1}{3} & \dfrac{4}{3} \\ -\dfrac{1}{3} & -\dfrac{1}{3} \end{pmatrix} \\\\ &= \begin{pmatrix} \dfrac{1 + 2^{13}}{3} & \dfrac{4 + 2^{13}}{3} \\ \dfrac{-1 + 2^{11}}{3} & \dfrac{-4 + 2^{11}}{3} \end{pmatrix} \end{align*}
$$
