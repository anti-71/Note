---
名称: LL1 解析表
章: 01 编译原理
节: "[[本科/大二下/01 编译原理/04 自顶向下分析/第四章 自顶向下分析]]"
tags:
  - 知识点
index: 6
---
**LL(1) 解析表（Parsing Table）** 是一个二维表，以非终结符为行、终结符（含 $\$$）为列，指导解析器在每一步应选择的产生式

##### 解析表的结构

| 非终结符 \ 终结符 | `(` | `)` | `$` |
|:---|:---|:---|:---|
| **S** | $S \to (S)S$ | $S \to \varepsilon$ | $S \to \varepsilon$ |

- **非空条目**：指导产生式替换
- **空条目**：表示语法错误（输入符号与预期不符）

##### 解析表的填充规则

构建 LL(1) 解析表 $M[A, a]$ 的规则：

1. **First 规则**：若 $a \in \text{First}(\alpha)$，则将 $A \to \alpha$ 加入 $M[A, a]$
2. **Follow 规则**：若 $\alpha \implies^* \varepsilon$ 且 $a \in \text{Follow}(A)$，则将 $A \to \alpha$ 加入 $M[A, a]$

##### 正则表达式文法的解析表

消除左递归后的表达式文法：
$$
\begin{align*}
exp  &\to term\ exp' \\
exp' &\to addop\ term\ exp' \mid \varepsilon \\
term &\to factor\ term' \\
term' &\to mulop\ factor\ term' \mid \varepsilon \\
factor &\to (exp) \mid number
\end{align*}
$$

| 非终结符 \ 终结符 | `(` | `number` | `)` | `+` | `-` | `*` | `$` |
|:---|:---|:---|:---|:---|:---|:---|:---|
| **exp** | exp → term exp' | exp → term exp' | - | - | - | - | - |
| **exp'** | - | - | exp' → ε | exp' → addop term exp' | exp' → addop term exp' | - | exp' → ε |
| **term** | term → factor term' | term → factor term' | - | - | - | - | - |
| **term'** | - | - | term' → ε | term' → ε | term' → ε | term' → mulop factor term' | term' → ε |
| **factor** | factor → (exp) | factor → number | - | - | - | - | - |

##### 悬挂 else 歧义性与 LL(1) 解析

**文法**：
$$
\begin{align*}
\text{Statement} &\to \text{if-stmt} \mid \text{other} \\
\text{if-stmt} &\to \text{if (exp) statement else-part} \\
\text{else-part} &\to \text{else statement} \mid \varepsilon \\
\text{exp} &\to 0 \mid 1
\end{align*}
$$

**歧义性**：当输入符号为 `else` 时，`else-part` 存在两个产生式选择：
- $\text{else-part} \to \text{else statement}$
- $\text{else-part} \to \varepsilon$

**解决规则**：**最接近嵌套原则**——优先匹配当前 `else`，选择 $\text{else-part} \to \text{else statement}$

> [!note]
