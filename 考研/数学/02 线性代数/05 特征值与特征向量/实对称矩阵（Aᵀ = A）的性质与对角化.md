---
名称: 实对称矩阵（Aᵀ = A）的性质与对角化
章: 第五章 特征值与特征向量
节: "[[第三节 实对称矩阵]]"
tags:
  - 知识点
index: 4
---
##### 1）实对称矩阵的特性 

###### 定理 1

实对称矩阵的特征值均为实数且必可对角化（不要求证明，背住即可）

> [!note]
> 两个实对称矩阵相似的充要条件是特征值相同

###### 定理 2

实对称矩阵对应于不同特征值的特征向量不但无关，且正交，即若 $A\boldsymbol{\alpha}_1 = \lambda_1\boldsymbol{\alpha}_1,A\boldsymbol{\alpha}_2 = \lambda_2\boldsymbol{\alpha}_2,\lambda_1 \neq \lambda_2$，则 $\boldsymbol{\alpha}_1^T\boldsymbol{\alpha}_2 = \boldsymbol{\alpha}_2^T\boldsymbol{\alpha}_1 = 0$

###### 定理 3

总存在正交矩阵 $Q$，使得 $Q^TAQ = Q^{-1}AQ = \text{diag}(\lambda_1,\lambda_2,\dots,\lambda_n)$

> [!note]
> 实对称矩阵的秩 = 非零特征值的个数

##### 2）实对称矩阵正交对角化的方法，即找 $Q$ 的方法

1. 求出 $A$ 的特征值 $\lambda$
2. 求出对应的线性无关特征向量 $\boldsymbol{\alpha}$（定有 $n$ 个）
3. 不同特征值对应特征向量已经正交，故仅需对同一个多重特征值对应的特征向量施密特正交化
4. 把所有已经正交的特征向量单位化，即可拼得 $Q$

显然，$Q$ 不唯一

> [!note]
> 在找 $Q$ 的过程中，施密特正交化只能对同一多重特征值对应的特征向量使用

###### 推论

两个实对称矩阵合同的充要条件是特征值的正、负号个数相同，即若 $A,B$ 为同阶实对称矩阵，则 $A \simeq B \iff \lambda_A$ 与 $\lambda_B$ 的正负号个数相同