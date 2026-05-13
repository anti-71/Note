---
名称: 求解方阵高次幂 Aⁿ 的方法总结
章: 第一章 矩阵
节: "[[强化题型总结 - 1]]"
tags:
  - 知识点
index: 7
---
1. 若矩阵 $\boldsymbol{A}$ 为对角矩阵或分块对角矩阵，则可以直接利用公式计算 $\boldsymbol{A}^n$
2. 若矩阵 $\boldsymbol{A}$ 中零元素较多，或元素分布有一定规律性，则可以先求矩阵 $\boldsymbol{A}$ 的低次幂分析出 $n$ 次幂的规律，最后用数学归纳法证明
3. 主对角线上元素全部为 0 的 $n$ 阶上（下）三角矩阵的 $n$ 次幂为零矩阵
4. 如果方阵 $\boldsymbol{A}$（秩为 1 的方阵）可以拆成一非零列 $\boldsymbol{\alpha}$ 与一非零行 $\boldsymbol{\beta}^\mathrm{T}$ 的乘积，那么可以利用以下公式计算：

$$ \begin{aligned} \boldsymbol{A}^n&=(\boldsymbol{\alpha}\boldsymbol{\beta}^\mathrm{T})^n\\
&\xlongequal{\text{结合律}}\boldsymbol{\alpha}(\boldsymbol{\beta}^\mathrm{T}\boldsymbol{\alpha})(\boldsymbol{\beta}^\mathrm{T}\boldsymbol{\alpha})\cdots(\boldsymbol{\beta}^\mathrm{T}\boldsymbol{\alpha})\boldsymbol{\beta}^\mathrm{T}\\ &=\boldsymbol{\alpha}(\boldsymbol{\beta}^\mathrm{T}\boldsymbol{\alpha})^{n-1}\boldsymbol{\beta}^\mathrm{T}\\
&=(\boldsymbol{\beta}^\mathrm{T}\boldsymbol{\alpha})^{n-1}\boldsymbol{\alpha}\boldsymbol{\beta}^\mathrm{T}\\ 
&=[\mathrm{tr}(\boldsymbol{A})]^{n-1}\boldsymbol{A}\end{aligned} $$

5. 若有 $\boldsymbol{P}^{-1}\boldsymbol{A}\boldsymbol{P}=\boldsymbol{\Lambda}$（其中 $\boldsymbol{\Lambda}$ 为对角矩阵，称 $\boldsymbol{A}$ 可对角化），则 $\boldsymbol{A}=\boldsymbol{P}\boldsymbol{\Lambda}\boldsymbol{P}^{-1}$，故 $\boldsymbol{A}^n=\boldsymbol{P}\boldsymbol{\Lambda}^n\boldsymbol{P}^{-1}$