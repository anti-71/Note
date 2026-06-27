---
名称: Follow 集
章: 01 编译原理
节: "[[本科/大二下/01 编译原理/04 自顶向下分析/第四章 自顶向下分析]]"
tags:
  - 知识点
index: 3
---
**Follow 集合** 表示在语法推导过程中，可能紧跟在某个非终结符**右侧**出现的所有终结符（包括输入结束符 $\$$）

> [!note]

##### 核心定义

对于非终结符 $A$，其 Follow 集合包含所有可能紧跟在 $A$ 右侧的终结符（包括输入结束符 $\$$）

##### 规则详解

##### 规则 1：初始符号规则

若 $A$ 是文法的**开始符号**，则 $\$ \in \text{Follow}(A)$

##### 规则 2：直接后继规则

若存在产生式 $B \to \alpha A \gamma$（其中 $\alpha, \gamma$ 为任意符号串），则：
$$
\text{Follow}(A) \cup= \text{First}(\gamma) - \{\varepsilon\}
$$

##### 规则 3：空传递规则

若上述 $\gamma$ 可推导出空串（即 $\varepsilon \in \text{First}(\gamma)$），则：
$$
\text{Follow}(A) \cup= \text{Follow}(B)
$$

##### Follow 集合算法

```plaintext
算法：计算所有非终结符的 Follow 集合

1. 初始化：
   - Follow(开始符号) = {$}
   - 其他非终结符的 Follow 集合初始化为空集 {}

2. 循环更新：
   while 任意 Follow 集合发生改变 do
       for 每个产生式 A → X₁X₂...Xₙ do
           for 每个非终结符 Xᵢ (1 ≤ i ≤ n) do
               // 规则2：添加直接后继符号
               γ = Xᵢ₊₁Xᵢ₊₂...Xₙ
               将 First(γ) - {ε} 加入 Follow(Xᵢ)

               // 规则3：处理 γ 可空的情况
               if ε ∈ First(γ) then
                   将 Follow(A) 加入 Follow(Xᵢ)
```

##### 求法

1. **开始符**：文法的开始符号加入 $\$$
2. 所要求字符为 $A$，遍历所有产生式右部
3. 若右部 $A$ 后面有东西（该东西不能推导出 $\varepsilon$；若能，规则 3 也适用），将 $\text{First}(该东西)$ 中**扣除 $\varepsilon$** 后加入 $\text{Follow}(A)$
4. 若右部 $A$ 后面没有东西，将 $\text{Follow}(左部)$ 加入 $\text{Follow}(A)$

##### 关键性质

- **符号限制**：Follow 集合仅针对非终结符，且不包含 $\varepsilon$
- **唯一来源**：输入结束符 $\$$

仅在初始符号的 Follow 集合中出现
- **方向性**：Follow 集合关注**右侧上下文**，而 First 集合关注**左侧推导**

##### Follow 集示例

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
| 非终结符 | Follow 集 |
|:---|:---|
| $E$ | $\{\,),\;\$\,\}$ |
| $E'$ | $\{\,),\;\$\,\}$ |
| $T$ | $\{\,+,\;),\;\$\,\}$ |
| $T'$ | $\{\,+,\;),\;\$\,\}$ |
| $F$ | $\{\,+,\;*,\;),\;\$\,\}$ |
