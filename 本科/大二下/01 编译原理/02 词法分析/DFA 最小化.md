---
名称: DFA 最小化
章: 01 编译原理
节: "[[第二章 词法分析]]"
tags:
  - 知识点
index: 2
---
##### DFA 最小化概述

**DFA 最小化**（DFA Minimization）是将 DFA 的状态数减少到最少的过程，消除**不可区分状态**（Indistinguishable States），得到等价但状态最少的 DFA

- **最小 DFA**：对每个正则语言，存在唯一的状态数最少的 DFA（不计状态命名）
- **最小化的意义**：节省存储空间、提高运行效率

##### 可区分状态与不可区分状态

- **区分**（Distinguishable）：存在某个字符串 w，使得从状态 p 和 q 分别出发读入 w 后，**一个到达接受状态而另一个不达**，则 p 和 q 可区分
- **等价**（Equivalent / Indistinguishable）：若对任意字符串 w，从 p 和 q 出发读入 w 后的接受性都相同，则 p 和 q 不可区分

##### 最小化算法：Hopcroft 算法

Hopcroft 算法是经典的 DFA 最小化算法，通过反复**划分**（Partition Refinement）来寻找等价状态：

```python
def minimize_dfa(dfa):
    """Hopcroft 的 DFA 最小化算法"""
    # 步骤 1：初始划分——接受状态 vs 非接受状态
    accept = frozenset(dfa.accept_states)
    non_accept = frozenset(dfa.states - accept)
    partition = [accept, non_accept] if non_accept else [accept]
    
    # 步骤 2：反复细化划分
    changed = True
    while changed:
        changed = False
        new_partition = []
        
        for group in partition:
            # 尝试将组内状态进一步划分
            subgroups = split_group(group, dfa, partition)
            if len(subgroups) > 1:
                changed = True
            new_partition.extend(subgroups)
        
        partition = new_partition
    
    # 步骤 3：构造最小 DFA
    return build_min_dfa(dfa, partition)


def split_group(group, dfa, current_partition):
    """将一个状态组按转移行为拆分为子组"""
    # 对每个输入字符，记录状态转移到哪个组
    signatures = {}
    for state in group:
        sig = []
        for char in dfa.alphabet:
            next_state = dfa.transitions[state][char]
            # 找到 next_state 属于哪个组
            for i, g in enumerate(current_partition):
                if next_state in g:
                    sig.append(i)
                    break
        signatures.setdefault(tuple(sig), []).append(state)
    
    return [frozenset(s) for s in signatures.values()]
```

##### 算法步骤详解

**步骤 1 — 初始划分：** 将所有状态分为两组——接受状态组和非接受状态组

**步骤 2 — 细分：** 对每组中的状态，检查它们在每个输入字符上转移到哪个组若组内状态转移到的组不同，则分裂该组

**步骤 3 — 重复：** 不断重复步骤 2，直到每组内状态完全等价（不能再分裂）

**步骤 4 — 构建最小 DFA：** 用划分后的每个组作为一个新状态，构建最小 DFA

##### 最小化示例

原始 DFA（7 个状态）→ 最小 DFA（4 个状态）：

```text
原始划分: {q₀, q₁, q₂, q₃, q₄}  {q₅, q₆}  (非接受 | 接受)
第一次迭代: {q₀, q₁, q₂}  {q₃, q₄}  {q₅, q₆}
第二次迭代: {q₀}  {q₁, q₂}  {q₃, q₄}  {q₅, q₆}
  → 无法再分裂，算法终止
```

##### 最小化的意义

```text
Thompson NFA:      12 个状态
子集构造法 DFA:    8 个状态
最小化 DFA:        3 个状态
```

**典型结果**：对词法分析中的实际正则表达式，最小化通常能显著减少 DFA 状态数（平均减少 30%–50%）

##### 完整流水线回顾

```text
  正则表达式 (RE)
       │
   ▼ Thompson构造法
   NFA (O(|r|) 状态)
       │
   ▼ 子集构造法
   DFA (最坏 O(2ⁿ) 状态)
       │
   ▼ DFA 最小化
   最优 DFA (最少状态)
       │
   ▼ 词法分析器生成
   转移表 / 代码
```

最小化是流水线的最后一步，确保最终生成的 DFA 是状态数最少的，从而让词法分析器占用最少的存储和最高的运行效率
