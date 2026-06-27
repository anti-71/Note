---
名称: 综合属性与 S 属性文法
章: 01 编译原理
节: "[[第六章 语义分析]]"
tags:
  - 知识点
index: 5
---
##### 综合属性与 S 属性文法

---

##### 综合属性的定义

**综合属性（Synthesized Attribute）** 是指属性值从**产生式的右部向左部传递**的属性即对于产生式 $A \to XYZ$，$A$ 的某个综合属性 $A.s$ 由 $X.s$、$Y.s$、$Z.s$ 或 $A$ 自身的其他属性计算得到

> [!abstract] 核心特征：**信息从子节点流向父节点**，对应语法树中自底向上的计算

- 综合属性的值在产生式右部的所有符号处理完毕后才能计算
- 综合属性是**唯一不依赖父节点或兄弟节点信息**的属性类型

---

##### 综合属性的计算时机

综合属性在**后序遍历（post-order traversal）** 中计算：

$$
\text{处理子节点} \to \text{使用子节点属性计算父节点属性}
$$

```
     A        ← 步骤 3: A.s = f(X.s, Y.s, Z.s)
    /|\
   X Y Z      ← 步骤 1-2: 先计算 X.s, Y.s, Z.s
```

---

##### 综合属性的典型应用

| 应用场景 | 综合属性 | 含义 |
|:---|:---|:---|
| 算术表达式求值 | $E.val$ | 子表达式的整数值 |
| 类型检查 | $E.type$ | 表达式的静态类型 |
| 中间代码生成 | $E.code$ | 子表达式的三地址码序列 |
| 常量折叠 | $E.constVal$ | 编译时可确定的常量值 |

---

##### 综合属性示例

文法：

$$
\begin{aligned}
E &\to E_1 + T \\
E &\to T \\
T &\to F \times T_1 \\
T &\to F \\
F &\to (E) \\
F &\to \text{num}
\end{aligned}
$$

综合属性 $E.val$ 的计算：

$$
\begin{aligned}
E &\to E_1 + T \quad &&\implies \quad E.val = E_1.val + T.val \\
E &\to T \quad &&\implies \quad E.val = T.val \\
T &\to F \times T_1 \quad &&\implies \quad T.val = F.val \times T_1.val \\
T &\to F \quad &&\implies \quad T.val = F.val \\
F &\to (E) \quad &&\implies \quad F.val = E.val \\
F &\to \text{num} \quad &&\implies \quad F.val = \text{num}.lexval
\end{aligned}
$$

对应表达式 $3 \times 5 + 4$ 的语法树后序遍历：

```
F.val = 4        T.val = 4
  ↓                 ↓
F.val = 5 → T.val = 5
  ↓              ↓     ↓
F.val = 3 → T.val = 3  E.val = 3  →  E.val = 3 × 5 = 15  →  E.val = 15 + 4 = 19
```

---

##### S-属性文法

**S-属性文法（S-Attributed Grammar）** 是仅使用**综合属性**的属性文法

> [!note]
> 在 S-属性文法中，语义规则都形如 $A.s = f(X_1.a_1, X_2.a_2, \dots, X_n.a_n)$，即左部符号的综合属性仅依赖右部符号的属性

**S-属性文法的特性：**

- 不需要显式的依赖图分析即可计算——属性依赖天然与语法树的**后序遍历**一致
- 可以在 **LR 分析器（如 Yacc/Bison）** 中通过在规约时执行语义动作直接实现
- 适用范围有限——许多语言的语义（如变量声明确定类型、作用域信息传递）需要**继承属性**

---

##### S-属性文法的实现方式

```
产生式                   语义动作（综合属性计算）
────────────────────────────────────────────
E → E + T                { E.val = E₁.val + T.val }
E → T                    { E.val = T.val }
T → num                  { T.val = num.lexval }
```

> [!tip] 在 **Yacc / Bison** 中，每个文法符号的

$$

表示左部的综合属性，$$

表示右部第 $i$ 个符号的综合属性

```yacc
expr: expr '+' term    { $$ = $1 + $3; }
    | term             { $$ = $1; }
    ;

term: NUMBER           { $$ = $1; }
    ;
```

- S-属性文法是属性文法中**最简单、最高效**的子类
- 但仅靠综合属性无法处理需要**上下文信息**的语义约束，如"变量声明后才能使用"——这需要继承属性

- #### [[继承属性与 L 属性文法]]
