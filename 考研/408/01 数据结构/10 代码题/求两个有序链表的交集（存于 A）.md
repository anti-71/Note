---
名称: 求两个有序链表的交集（存于 A）
章: 代码题
节: "[[第二节 链表]]"
tags:
  - 知识点
index: 9
---
##### 问题描述

> [!question] 
> 已知两个链表 A 和 B 分别表示两个集合，其元素递增排列，编制函数，求 A 与 B 的交集，并存放于 A 链表中

##### 算法思想

采用归并思想，设置两个工作指针 p1（A）和 p2（B）同步扫描：值相等则保留 A 中的结点（即为交集元素），两者后移；A 值小则删除 A 中结点；B 值小则 p2 后移

循环结束后，截断 A 中剩余结点

最终 A 中仅保留交集元素

##### 代码实现

```cpp
void Union(LinkList &A, LinkList &B) {
    LNode *p1_pre = A.head, *p1 = A.head->next, *p2 = B.head->next;
    while (p1 != NULL && p2 != NULL) {
        if (p1->data == p2->data) {
            p1_pre = p1_pre->next;
            p1 = p1->next;
            p2 = p2->next;
        }
        else if (p1->data < p2->data) {
            LNode *temp = p1;
            p1_pre->next = p1->next;
            p1 = p1->next;
            free(temp);
        }
        else
            p2 = p2->next;
    }
    if (p1 != NULL)
        p1_pre->next = NULL;
}
```

##### 复杂度分析

- **时间复杂度：** $O(m + n)$ — 两表各扫描一遍
- **空间复杂度：** $O(1)$ — 未申请新结点，仅释放结点

##### 参考答案

```cpp
LinkList Union(LinkList &la, LinkList &lb) {
    LNode *pa = la->next, *pb = lb->next;
    LNode *u, *pc = la;                 // pc 为结果表中当前合并结点的前驱指针
    while (pa && pb) {
        if (pa->data == pb->data) {     // 交集并入结果表
            pc->next = pa;
            pc = pa;
            pa = pa->next;
            u = pb;                     // B 中结点释放
            pb = pb->next;
            free(u);
        }
        else if (pa->data < pb->data) { // A 值小于 B 值
            u = pa;
            pa = pa->next;
            free(u);                    // 释放 A 中当前结点
        }
        else {
            u = pb;
            pb = pb->next;
            free(u);                    // 释放 B 中当前结点
        }
    }
    while (pa) {                        // B 已遍历完，A 未完
        u = pa;
        pa = pa->next;
        free(u);                        // 释放 A 中剩余结点
    }
    while (pb) {                        // A 已遍历完，B 未完
        u = pb;
        pb = pb->next;
        free(u);                        // 释放 B 中剩余结点
    }
    pc->next = NULL;                    // 置结果链表尾指针为 NULL
    free(lb);                           // 释放 B 表的头结点
    return la;
}
```

##### 补充说明

你的交集结果正确（A 中仅保留两表公共元素），但存在两处内存未释放的问题：

1. 循环结束后 A 中剩余结点仅被截断（`p1_pre->next = NULL`）而未 free；
2. B 表所有结点（含头结点）均未释放
   
   参考答案将 B 整表释放、A 剩余结点逐个释放

##### 评分分析

**评分：8 / 10**

| 方面 | 得分 | 说明 |
|------|:----:|------|
| 交集结果正确 | ✅ 满分 | A 中正确保留两表公共元素 |
| 归并算法 | ✅ 满分 | $O(m + n)$ 扫描，思路正确 |
| 内存释放 | ❌ 扣 2 分 | ①A 尾部剩余结点被截断后未 free（内存泄漏）；②B 表全部结点及头结点未释放 |

##### 修改建议

1. **释放 A 尾部剩余结点**：将 `if (p1 != NULL) p1_pre->next = NULL;` 改为循环释放 p1 起的全部结点
2. **释放 B 表**：遍历 B 逐个 free 所有结点，最后释放 B 的头结点（参考参考答案的两个 `while` 循环）
