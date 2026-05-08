---
名称: 方阵高次幂 Aⁿ 的求解
章: 第一章 矩阵
节: "[[第六节 方阵高次幂 Aⁿ 的求解专题]]"
tags:
  - 知识点
index: 1
---
1. 若矩阵 $A$ 为对角矩阵或分块对角矩阵，则可以直接利用公式计算 $A^n$
2. 若矩阵 $A$ 中零元素较多，或元素分布有一定规律性，则可以先求矩阵 $A$ 的低次幂，分析出 $n$ 次幂的规律，最后用数学归纳法证明
3. 主对角线上元素全部为 0 的 $n$ 阶上（下）三角矩阵的 $n$ 次幂为零矩阵
4. 如果方阵 $A$ 可以拆成一非零列 $\alpha$ 与一非零行 $\beta^T$ 的乘积（即秩为 1 的方阵），那么可以利用以下公式计算：
   $$
    \begin{align*} A^n &= (\alpha\beta^T)^n = (\alpha\beta^T)(\alpha\beta^T)\cdots(\alpha\beta^T) \quad (\text{共}n\text{个}) \\ &= \alpha(\beta^T\alpha)(\beta^T\alpha)\cdots(\beta^T\alpha)\beta^T \\ &= (\beta^T\alpha)^{n-1}\alpha\beta^T = (\beta^T\alpha)^{n-1}A \\
   &= [\text{tr}(A)]^{n-1}A \end{align*} 
   $$

5. 若有 $P^{-1}AP = \Lambda$（其中 $\Lambda$ 为对角矩阵，若等式成立，则称 $A$ 可对角化），则 $A = P\Lambda P^{-1}$，故 $A^n = P\Lambda^n P^{-1}$