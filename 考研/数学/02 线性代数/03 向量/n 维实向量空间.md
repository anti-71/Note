---
名称: n 维实向量空间
章: 第三章 向量
节: "[[第五节 向量空间]]"
tags:
  - 知识点
index: 5
---
所有 $n$ 维实向量构成的集合是一个向量空间，称为 $n$ **维实向量空间**，记作 $\mathbb{R}^n$

显然，**对 $n$ 维向量空间来说，由于基形成的矩阵都是可逆方阵**，所以如果 $C$ 为从基 $\alpha_1,\alpha_2,\dots,\alpha_n$ 到基 $\beta_1,\beta_2,\dots,\beta_n$ 的过渡矩阵，则有下面两个结论：

1. $C = (\alpha_1,\alpha_2,\dots,\alpha_n)^{-1}(\beta_1,\beta_2,\dots,\beta_n)$； 
2. 任一向量 $\gamma$ 在 $\alpha_1,\alpha_2,\dots,\alpha_n$ 与 $\beta_1,\beta_2,\dots,\beta_n$ 两组基下的坐标 $(x_1,x_2,\dots,x_n)^T = X$ 与 $(y_1,y_2,\dots,y_n)^T = Y$ 的关系是：
   
   $X = CY \iff Y = C^{-1}X$