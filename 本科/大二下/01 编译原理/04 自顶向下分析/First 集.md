---
名称: First 集
章: 01 编译原理
节: "[[本科/大二下/01 编译原理/04 自顶向下分析/第四章 自顶向下分析]]"
tags:
  - 知识点
index: 2
---
**First 集合** 定义了从某个文法符号或符号串出发，通过一步或多步推导可以得到的**所有可能的首终结符**，以及可能的 $\varepsilon$

##### 基本定义

- 若 $X$ 是 **终结符**，则 $\text{First}(X) = \{X\}$
- 若 $X$ 是 **非终结符**，则 $\text{First}(X)$ 由其产生式递归决定

##### 字符串的 First 集合

对于符号串 $\alpha = X_1 X_2 \dots X_n$：

1. 初始添加 $\text{First}(X_1) - \{\varepsilon\}$
2. 若前 $j-1$ 个符号的 First 均含 $\varepsilon$，则添加 $\text{First}(X_j) - \{\varepsilon\}$（逐个检查后续符号）
3. 若所有符号的 First 均含 $\varepsilon$，则添加 $\varepsilon$

##### 可空非终结符（Nullable）

非终结符 $A$ 是**可空**的，当且仅当存在推导 $A \implies^* \varepsilon$

**定理**：
$$
A \text{ 是可空的} \iff \varepsilon \in \text{First}(A)
$$

##### First 集合算法

##### 完整算法（支持含 $\varepsilon$ 的产生式）

```plaintext
for all nonterminals A do First(A) := {}
while 任意 First(A) 发生改变 do
    for 每个产生式 A → X₁X₂...Xₙ do
        k := 1; Continue := true
        while Continue and k ≤ n do
            将 First(Xₖ) - {ε} 加入 First(A)
            if ε ∉ First(Xₖ) then Continue := false
            k := k + 1
        if Continue == true then 将 ε 加入 First(A)
```

逐个检查产生式右部符号，若某个符号不含 $\varepsilon$，停止检查后续符号；若所有符号均含 $\varepsilon$，最终添加 $\varepsilon$

##### 简化算法（无 $\varepsilon$ 产生式）

```plaintext
for all nonterminals A do First(A) := {}
while 任意 First(A) 发生改变 do
    for 每个产生式 A → X₁X₂...Xₙ do
        将 First(X₁) 加入 First(A)
```

当文法中无显式或隐式的 $\varepsilon$ 产生式时，直接取右部第一个符号的 First 集合

##### 求法

1. 遍历以所要求字符 $A$ 为左部的所有产生式
2. 若产生式右部首字符为**终结符**，加入到 $\text{First}(A)$ 中
3. 若为**非终结符**，遍历以该字符为左部的产生式，将所得到的首字符终结符加入到 $\text{First}(A)$ 中
4. 若有 $\varepsilon$，也加入到 $\text{First}(A)$ 中

##### First 集示例

**文法**：
$$
\begin{align*}
E  &\to T E' \\
E' &\to + T E' \mid \varepsilon \\
T  &\to F T' \\
T' &\to * F T' \mid \varepsilon \\
F  &\to (E) \mid id
\end{align*}
$$

计算结果：
| 非终结符 | First 集 |
|:---|:---|
| $E$ | $\{\,(\,,\;id\,\}$ |
| $E'$ | $\{\,+,\;\varepsilon\,\}$ |
| $T$ | $\{\,(\,,\;id\,\}$ |
| $T'$ | $\{\,*,\;\varepsilon\,\}$ |
| $F$ | $\{\,(\,,\;id\,\}$ |
