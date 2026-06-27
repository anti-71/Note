---
名称: DFA
章: 01 编译原理
节: "[[第二章 词法分析]]"
tags:
  - 知识点
index: 1
---
##### DFA 的定义

**确定性有限自动机**（Deterministic Finite Automaton, DFA）是每个状态对每个输入字符都有且只有一个确定转移的有限自动机

DFA 的形式化定义：

```
M = (Q, Σ, δ, q₀, F)
```

其中转移函数 **δ: Q × Σ → Q** 是一个**完全定义**的确定函数

##### DFA 的三个关键性质

1. **确定性**：对每个状态和每个输入符号，至多只有一个转移
2. **完全定义**：转移函数对所有输入符号都有定义（不缺失）
3. **无 ε 转移**：DFA 不包含空串转移

##### DFA 的接受条件

DFA 接受字符串 **s** 当且仅当从初始状态开始，**唯一一条**沿 s 标记的路径最终到达接受状态：

```text
δ*(q₀, s) ∈ F
```

其中 δ* 是 δ 的扩展，作用于字符串而非单字符

##### DFA 的转移表表示

DFA 通常用**转移表**（Transition Table）表示，行是状态，列是输入符号：

```text
      |  a    b
──────┼────────
→ q₀  | q₁   q₂
  q₁  | q₀   q₃
* q₂  | q₃   q₀
  q₃  | q₃   q₃
```

- `→` 表示初始状态
- `*` 表示接受状态

##### DFA 的模拟算法

DFA 的模拟算法非常高效，每个字符只需一步查找：

```python
def simulate_dfa(dfa, input_string):
    current_state = dfa.start_state
    
    for char in input_string:
        if char not in dfa.transitions[current_state]:
            return False  # 无转移 → 拒绝
        current_state = dfa.transitions[current_state][char]
    
    return current_state in dfa.accept_states
```

##### DFA 的优缺点

| 优点 | 缺点 |
|-----|------|
| 运行效率高（O(n) 时间） | 状态数可能很大（相对 NFA） |
| 实现简单（查表即可） | 对某些正则语言，状态数指数增长 |
| 确定性，无回溯 | 设计和构造较复杂 |

##### 词法分析器中的 DFA

在实际的词法分析器中，DFA 是**最终的执行形态**——词法分析器在运行时模拟 DFA 扫描输入，每遇到一个接受状态就记录当前位置，最终取**最长匹配**作为当前词素

```c
// 词法分析器核心循环的简化伪代码
state = start_state;
while (state && !is_accept(state)) {
    state = transition[state][next_char()];
    if (is_accept(state))
        mark_token_start(last_accept_pos);
}
```
