---
名称: LR0 项目的有穷自动机
章: 01 编译原理
节: "[[本科/大二下/01 编译原理/05 自底向上语法分析/第五章 自底向上语法分析]]"
tags:
  - 知识点
index: 4
---
LR(0) 项目的**有限自动机**用于跟踪语法分析栈的状态和移进-归约的执行进度最初为 **NFA**，可通过子集构造法转换为 **DFA**

##### NFA 的转移规则

##### 1. 基础转移（对任意符号 $X$）

若项目为 $A \to \alpha \cdot X\eta$，则存在符号 $X$ 上的转移：

$$
\overset{A \to \alpha \cdot X\eta}{\circ} \quad \xrightarrow{X} \quad \overset{A \to \alpha X \cdot \eta}{\circ}
$$

##### 2. 按符号类型分类

- **当 $X$ 是终结符**：转移对应**移进操作**，将输入中的 $X$ 移至分析栈顶
- **当 $X$ 是非终结符**：需通过归约生成，添加 **$\epsilon$-转移**指向 $X$ 的所有初始项

$$
\begin{cases}
\overset{A \to \alpha \cdot X\eta}{\circ} \quad \xrightarrow{\varepsilon} \quad \overset{X \to \cdot \beta}{\circ} \quad \text{（为每个 } X \to \beta \text{ 添加）} \\
\overset{A \to \alpha \cdot X\eta}{\circ} \quad \xrightarrow{X} \quad \overset{A \to \alpha X \cdot \eta}{\circ} \quad \text{（基础转移）}
\end{cases}
$$

##### 初始状态与增广文法

NFA 的初始状态为增广产生式的初始项 $S' \to \cdot S$，表示即将开始识别原文法的 $S$

##### DFA 构造方法

1. **文法准备**：将文法转换为增广文法（添加 $S' \to S$）
2. **画 LR(0) DFA**：
   - 项目 0 为增广文法的初始项 $S' \to \cdot S$
   - 对每个项目状态：若圆点后是非终结符，在本项目增加该非终结符的所有初始项（求 $\epsilon$-闭包）
   - 输入每一个项目圆点后的符号，转到下一个状态

> [!note] 求法要点

##### NFA 到 DFA 的核心规则

| 规则 | 处理方式 |
|------|----------|
| 基础转移 | 圆点后移一位 |
| $\epsilon$-转移 | 非终结符初始项的闭包 |
| 增广文法 | 统一初始状态 |
