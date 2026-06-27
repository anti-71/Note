---
名称: NFA
章: 01 编译原理
节: "[[第二章 词法分析]]"
tags:
  - 知识点
index: 3
---
##### NFA 的定义

**非确定性有限自动机**（Nondeterministic Finite Automaton, NFA）是对同一输入字符允许有**多个转移**且支持 **ε 转移**（空串转移）的有限自动机

NFA 的形式化定义：

```
M = (Q, Σ, δ, q₀, F)
```

其中转移函数 **δ: Q × (Σ ∪ {ε}) → 2^Q** 是状态集合的幂集函数

##### NFA 与 DFA 的关键区别

| 特性 | NFA | DFA |
|-----|-----|-----|
| 转移确定性 | 非确定性（多个选择） | 确定性（唯一选择） |
| ε 转移 | 支持（不消耗输入字符） | 不支持 |
| 转移函数值域 | 状态集合（2^Q） | 单个状态（Q） |
| 分支 | 可同时在多个状态 | 始终在唯一状态 |

##### NFA 的非确定性含义

NFA 接受字符串 **s** 当且仅当**存在至少一条**从初始状态到接受状态的路径，其标记序列等于 s：

```text
输入: "ab"
NFA 可能的路径:
┌─路径1: q₀ →(a)→ q₁ →(b)→ q₂ (接受) ✓
├─路径2: q₀ →(a)→ q₃ →(ε)→ q₄ →(b)→ q₅ (拒绝) ✗
└─路径3: q₀ →(ε)→ q₆ →(a)→ q₇ →(b)→ q₈ (接受) ✓

只要有至少一条路径到接受状态 → 接受该串
```

##### ε 转移的作用

**ε 转移**（空串转移）允许自动机在不消耗输入字符的情况下改变状态其主要用途：

1. **连接** NFA 片段（在 Thompson 构造法中）
2. 表达"可选"路径
3. 简化自动机的结构

```text
示例：a | b 的 NFA

      ε
    ┌───→ [NFA for a]
    │
→ q₀ ───→ [NFA for b]
      ε
```

##### NFA 的模拟算法

NFA 的模拟需要维护**当前可能的状态集合**，而不是单个状态：

```python
def simulate_nfa(nfa, input_string):
    # 初始状态集的 ε-闭包
    current_states = epsilon_closure({nfa.start_state})
    
    for char in input_string:
        # 对当前状态集中每个状态应用 char 转移
        next_states = set()
        for state in current_states:
            if char in nfa.transitions[state]:
                next_states.update(nfa.transitions[state][char])
        # 计算 ε-闭包
        current_states = epsilon_closure(next_states)
    
    return any(s in nfa.accept_states for s in current_states)

def epsilon_closure(states):
    """计算状态集合的 ε-闭包（所有可通过 ε 转移到达的状态）"""
    stack = list(states)
    closure = set(states)
    while stack:
        state = stack.pop()
        for next_state in nfa.epsilon_transitions[state]:
            if next_state not in closure:
                closure.add(next_state)
                stack.append(next_state)
    return closure
```

##### NFA 的优缺点

| 优点 | 缺点 |
|-----|------|
| 构造直观简单（与正则表达式结构对应） | 模拟效率较低（维护状态集） |
| 状态数通常比等价 DFA 少 | 需要 ε 闭包计算 |
| 方便理论分析和证明 | 非确定性难以直接实现 |

##### NFA 与 DFA 的等价性

**定理**：对每个 NFA，都存在一个等价的 DFA（通过[[子集构造法]]）反之亦然

- NFA 和 DFA 都识别**正则语言**
- NFA 更易于构造（直接对应于正则表达式）
- DFA 更易于执行（确定性的查表）
- 编译器实践中：正则 → NFA（Thompson） → DFA（子集构造法）
