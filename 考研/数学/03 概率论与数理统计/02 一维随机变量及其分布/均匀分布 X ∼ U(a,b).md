---
名称: 均匀分布 X ∼ U(a,b)
章: 第二章 一维随机变量及其分布
节: "[[第四节 连续型随机变量的概率密度与常见分布]]"
tags:
  - 知识点
index: 2
---
##### 概率密度

$$
 f(x)= \begin{cases} \dfrac{1}{b - a},& a \leqslant x \leqslant b\\\\ 0,& 其他 \end{cases}
$$

##### 分布函数

$$
F(x)= \begin{cases} 0,& x < a\\\\ \dfrac{x - a}{b - a}，& a \leqslant x < b\\\\ 1,& x \geqslant b \end{cases}
$$

> [!note]
> 若 $X \sim U(a,b)$，$[c,d] \subset [a,b]$，则 $P(c < X < d) = \dfrac{d - c}{b - a}$