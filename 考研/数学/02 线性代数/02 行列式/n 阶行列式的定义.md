---
名称: n 阶行列式的定义
章: 第二章 行列式
节: "[[第一节 行列式的定义]]"
tags:
  - 知识点
index: 2
---
设 $A$ 为一个 $n$ 阶方阵，$A$ 的行列式

$$
 \det A = |A| = \begin{vmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\ a_{21} & a_{22} & \cdots & a_{2n} \\ \vdots & \vdots & & \vdots \\ a_{n1} & a_{n2} & \cdots & a_{nn} \end{vmatrix} 
$$

是所有取自不同行不同列的 $n$ 个元素的乘积

$$
a_{1j_1}a_{2j_2}\cdots a_{nj_n}
$$

的代数和，这里 $j_1j_2\cdots j_n$ 表示自然数 $1,2,\dots,n$ 的一个排列

- 当 $j_1j_2\cdots j_n$ 是偶排列时，该项前面带正号
- 当 $j_1j_2\cdots j_n$ 是奇排列时，该项的前面带负号

即

$$
\det A = |A| = \begin{vmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\ a_{21} & a_{22} & \cdots & a_{2n} \\ \vdots & \vdots & & \vdots \\ a_{n1} & a_{n2} & \cdots & a_{nn} \end{vmatrix} = \sum_{j_1j_2\cdots j_n} (-1)^{\tau(j_1j_2\cdots j_n)} a_{1j_1}a_{2j_2}\cdots a_{nj_n}, \tag{2.1} 
$$

这里 $\sum\limits_{j_1j_2\cdots j_n}$ 表示对所有 $n$ 阶排列求和，式 (2.1) 称为 $n$ 阶行列式的 **完全展开式**

> [!warning] 注意
> 1. 只有 **方阵** 才有行列式，其他类型矩阵没有行列式的说法
> 2. 行列式是个 **常数**，即 $|A| = c$
