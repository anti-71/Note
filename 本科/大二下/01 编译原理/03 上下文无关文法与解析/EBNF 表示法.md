---
名称: EBNF 表示法
章: 01 编译原理
节: "[[本科/大二下/01 编译原理/03 上下文无关文法与解析/第三章 上下文无关文法与解析]]"
tags:
  - 知识点
index: 1
---
**EBNF**（Extended BNF，扩展巴科斯范式）在 BNF 的基础上增加了用于表示**重复**和**可选**结构的特殊符号，使文法更加简洁

##### 重复结构

BNF 中的递归需要通过左递归或右递归来实现重复：

- **左递归**：$A \to A\alpha \mid \beta$
- **右递归**：$A \to \alpha A \mid \beta$

其中 $\alpha$ 和 $\beta$ 是任意的终结符和非终结符的字符串

EBNF 使用**花括号** `{...}` 来表示重复结构：

- **左递归的 EBNF 表示**：$A \to \beta \{\alpha\}$
- **右递归的 EBNF 表示**：$A \to \{\alpha\} \beta$

##### 可选结构

EBNF 使用**方括号** `[...]` 来表示可选结构

例如，if 语句的文法规则可以写成：

```bnf
if-stmt → if ( exp ) statement [else statement]
```

##### 结合性在 EBNF 中的表示

EBNF 还可以表示结合性：

- **左结合**：$exp \to term\ \{addop\ term\}$
- **右结合**：$exp \to \{term\ addop\}\ term$

> [!note] EBNF 的优势
