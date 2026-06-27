---
名称: LL1 分析表构建
章: 01 编译原理
节: "[[本科/大二下/01 编译原理/04 自顶向下分析/第四章 自顶向下分析]]"
tags:
  - 知识点
index: 4
---
**LL(1) 分析表** 的构建基于 **First 集合** 和 **Follow 集合**，是 LL(1) 解析器的核心数据结构

##### 构建规则

对每个非终结符 $A$ 及其产生式 $A \to \alpha$，按以下规则填充分析表 $M[A, a]$：

##### 基于 First 集合

对所有终结符 $a \in \text{First}(\alpha)$，将产生式 $A \to \alpha$ 填入表条目 $M[A, a]$

##### 基于 Follow 集合（若 $\alpha$ 可推导为空）

若 $\varepsilon \in \text{First}(\alpha)$，则对所有终结符 $a \in \text{Follow}(A)$，将 $A \to \alpha$ 填入 $M[A, a]$

##### 算法实现

```plaintext
for 每个非终结符 A 和其产生式 A → α do:
    // 步骤1：处理 First(α) 中的终结符
    for 每个终结符 a ∈ First(α) do:
        将 A → α 加入 M[A, a]
    
    // 步骤2：处理 α 可推导为空的情况
    if ε ∈ First(α) then:
        for 每个终结符 a ∈ Follow(A) do:
            将 A → α 加入 M[A, a]
```

##### 求法

1. 左边一列为非终结符，上方一行为终结符和 "$\$$"
2. 遍历每一个产生式
3. 若产生式里没有 $\varepsilon$，若 $\text{First}(左边) = \{a\}$，$a$ 为产生式推出的唯一终结符，$M[左边, a]$ 添加该产生式
4. 若产生式里有 $\varepsilon$，看 $\text{Follow}(左边) = \{a\}$，$M[左边, a]$ 添加该产生式

##### 构建完整示例

##### 文法

消除左递归后的表达式文法：
$$
\begin{align*}
E  &\to T E' \\
E' &\to + T E' \mid \varepsilon \\
T  &\to F T' \\
T' &\to * F T' \mid \varepsilon \\
F  &\to (E) \mid id
\end{align*}
$$

##### First 集
| 非终结符 | First |
|:---|:---|
| $E$ | $\{(\,,\;id\}$ |
| $E'$ | $\{+\,,\;\varepsilon\}$ |
| $T$ | $\{(\,,\;id\}$ |
| $T'$ | $\{*\,,\;\varepsilon\}$ |
| $F$ | $\{(\,,\;id\}$ |

##### Follow 集
| 非终结符 | Follow |
|:---|:---|
| $E$ | $\{\,),\;\$\}$ |
| $E'$ | $\{\,),\;\$\}$ |
| $T$ | $\{+\,,\;),\;\$\}$ |
| $T'$ | $\{+\,,\;),\;\$\}$ |
| $F$ | $\{+\,,\;*\,,\;),\;\$\}$ |

##### 应用填充规则

以产生式 $E \to T E'$ 为例：
- $\text{First}(T E') = \text{First}(T) = \{(\,,\;id\}$
- 在 $M[E, (]$ 和 $M[E, id]$ 中填入 $E \to T E'$

以产生式 $E' \to \varepsilon$ 为例：
- $\varepsilon \in \text{First}(\varepsilon)$，$\text{Follow}(E') = \{\,),\;\$\}$
- 在 $M[E', )]$ 和 $M[E', \$]$ 中填入 $E' \to \varepsilon$

##### 完整 LL(1) 分析表

| 非终结符 \ 终结符 | `(` | `id` | `)` | `+` | `*` | `$` |
|:---|:---|:---|:---|:---|:---|:---|
| **E** | $E \to T E'$ | $E \to T E'$ | - | - | - | - |
| **E'** | - | - | $E' \to \varepsilon$ | $E' \to + T E'$ | - | $E' \to \varepsilon$ |
| **T** | $T \to F T'$ | $T \to F T'$ | - | - | - | - |
| **T'** | - | - | $T' \to \varepsilon$ | $T' \to \varepsilon$ | $T' \to * F T'$ | $T' \to \varepsilon$ |
| **F** | $F \to (E)$ | $F \to id$ | - | - | - | - |

> [!note]
