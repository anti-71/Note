---
名称: 悬挂 else 问题
章: 01 编译原理
节: "[[本科/大二下/01 编译原理/03 上下文无关文法与解析/第三章 上下文无关文法与解析]]"
tags:
  - 知识点
index: 7
---
**悬挂的 else 问题（Dangling Else Problem）** 是 if 语句文法由于可选的 `else` 部分而产生的经典歧义问题

##### 问题描述

考虑以下文法：

```bnf
statement → if-stmt | other
if-stmt → if ( exp ) statement | if ( exp ) statement else statement
exp → 0 | 1
```

由于可选的 `else` 部分，这个文法产生歧义

##### 歧义示例

字符串 `if (0) if (1) other else other` 可以有两种不同的语法分析树：

1. **第一个分析树**：将 `else` 部分与第一个 `if` 语句关联
2. **第二个分析树**：将 `else` 部分与第二个 `if` 语句关联

##### 正确性判断

通常情况下，我们希望将 `else` 与**最近的没有 else 的 if 语句**关联这种去歧义规则被称为**最接近嵌套规则（Most Closely Nested Rule）**，表明第二个分析树是正确的

##### 修改后的文法

为解决悬挂的 else 问题，可以修改文法如下：

```bnf
statement → matched-stmt | unmatched-stmt
matched-stmt → if ( exp ) matched-stmt else matched-stmt | other
unmatched-stmt → if ( exp ) statement | if ( exp ) matched-stmt else unmatched-stmt
exp → 0 | 1
```

对于字符串 `if (0) if (1) other else other`，修改后的文法生成的解析树：

```
        statement
          |
    unmatched-stmt
      /   / | \        \
   if    ( exp )     statement
              |              |
              0        matched-stmt
                /  / /  /      |       \      \
               if ( exp ) matched-stmt else matched-stmt
                      |         |                  |
                      1        other             other
```

##### 语言设计层面的解决方案

悬挂的 else 问题起源于 Algol 60 的语法可以通过以下方法设计语法以**避免此问题**：

1. **要求存在 else 部分**：此方法已在 LISP 和其他函数式语言中使用（其中还必须返回一个值）
2. **使用括号关键字**：此方法在 Algol 68 和 Ada 等语言中使用（如 `if ... then ... else ... end if`）

> [!note] 悬挂 else 的普遍性
