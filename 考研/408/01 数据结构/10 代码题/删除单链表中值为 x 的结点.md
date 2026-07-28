---
名称: 删除单链表中值为 x 的结点
章: 代码题
节: "[[第二节 链表]]"
tags:
  - 知识点
index: 1
---
##### 问题描述

> [!question] 
> 在带头结点的单链表 L 中，删除所有值为 x 的结点，并释放其空间，假设值为 x 的结点不唯一，试编写算法以实现上述操作

##### 算法思想

用指针 p 从头结点开始遍历，每次检查 p->next 是否为待删结点：若是，则删除并 free，p 保持不动（下次检查新的 p->next）；若不是，则 p 后移。如此只需一次扫描即可完成

##### 代码实现

```cpp
void Delete_X(LinkList &L, ElemType x) {
    LNode *p = L.head;
    while (p->next != NULL) {
        if (p->next->data == x) {
            LNode *q = p->next;
            p->next = q->next;
            free(q);
        }
        else
            p = p->next;
    }
}
```

##### 复杂度分析

- **时间复杂度：** $O(n)$ — 只需一次扫描
- **空间复杂度：** $O(1)$ — 仅使用常数个辅助变量

##### 参考答案

解法一（双指针）：用 p 扫描，pre 指向 p 的前驱

```cpp
void Del_X_1(Linklist &L, ElemType x) {
    LNode *p = L->next, *pre = L, *q;
    while (p != NULL) {
        if (p->data == x) {
            q = p;
            p = p->next;
            pre->next = p;
            free(q);
        } else {
            pre = p;
            p = p->next;
        }
    }
}
```

解法二（尾插法）：将值不为 x 的结点重新链接到 L 后

```cpp
void Del_X_2(Linklist &L, ElemType x) {
    LNode *p = L->next, *r = L, *q;
    while (p != NULL) {
        if (p->data != x) {
            r->next = p;
            r = p;
            p = p->next;
        } else {
            q = p;
            p = p->next;
            free(q);
        }
    }
    r->next = NULL;
}
```

##### 补充说明

你的解法用一个指针 p（检查 p->next）替代了参考答案的双指针（pre + p），思路更简洁：删除时 p 不移动，自然就接续了前驱的位置，避免了额外维护 pre

两种写法时间复杂度相同

##### 评分分析

**评分：10 / 10**

| 方面 | 得分 | 说明 |
|------|:----:|------|
| 算法正确性 | ✅ 满分 | 能正确删除所有值为 x 的结点，释放空间 |
| 边界处理 | ✅ 满分 | 空链表（头结点后无结点）、连续多个 x、全部为 x 等情况均正确 |
| 代码简洁 | ✅ 满分 | 单指针遍历 p->next，比双指针法更精简 |
| 复杂度 | ✅ 满分 | $O(n)$ 时间，$O(1)$ 空间 |

##### 修改建议

无，代码已是最优方案

可注意 `L.head` 的写法依赖你的 `LinkList` 具体定义，若采用 408 标准定义（`typedef LNode *LinkList`），则直接用 `L` 即可（因 L 本身就是指向头结点的指针）
