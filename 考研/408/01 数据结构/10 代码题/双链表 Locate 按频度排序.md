---
名称: 双链表 Locate 按频度排序
章: 代码题
节: "[[第二节 链表]]"
tags:
  - 知识点
index: 13
---
##### 问题描述

> [!question] 
> 设有一个带头结点的非循环双链表 L，其每个结点中除有 pre、data 和 next 域外，还有一个访问频度域 freq，其值均初始化为零
> 
> 每当在链表中进行一次 Locate(L, x) 运算时，令值为 x 的结点中 freq 域的值增 1，并使此链表中的结点保持按访问频度递减的顺序排列，且最近访问的结点排在频度相同的结点之前，以便使频繁访问的结点总是靠近表头
> 
> 试编写符合上述要求的 Locate(L, x) 函数，返回找到结点的地址，类型为指针型

##### 算法思想

先在双链表中查找数据值为 x 的结点，查到后将其 freq 域加 1，并把结点从链表上摘下；然后顺着前驱链向前找到第一个频度大于它的结点，插入在该结点之后，从而保持频度递减且同频度新访问的在前

##### 代码实现

```cpp
DNode *Locate(DLinkList &L, ElemType x) {
    DNode *p = L.head->next, *q;
    while (p != NULL) {
        if (p->data == x) {
            p->freq++;
            q = p->pre;
            while (q->pre != NULL) {
                if (q->freq == p->freq) {
                    if (p->next != NULL) {
                        q->next = p->next;
                        p->next->pre = q;
                    }
                    else
                        q->next = NULL;
                    p->next = q;
                    p->pre = q->pre;
                    q->pre->next = p;
                    q->pre = p;
                    q = p->next;
                }
            }
            return p;
        }
        p = p->next;
    }
    return NULL;
}
```

##### 复杂度分析

- **时间复杂度：** $O(n)$ — 查找结点 $O(n)$，向前移动最多 $n$ 次，总计 $O(n)$
- **空间复杂度：** $O(1)$ — 仅使用常数个辅助变量

##### 参考答案

```cpp
DLinkList Locate(DLinkList &L, ElemType x) {
    DNode *p = L->next, *q;        // p 为工作指针，q 用于查找插入位置
    while (p && p->data != x)
        p = p->next;               // 查找值为 x 的结点
    if (!p)
        exit(0);                   // 不存在值为 x 的结点
    else {
        p->freq++;                 // 令 freq 域加 1
        if (p->pre == L || p->pre->freq > p->freq)
            return p;              // p 是首结点，或 freq 值小于前驱，无需移动
        if (p->next != NULL) p->next->pre = p->pre;
        p->pre->next = p->next;    // 将 p 结点从链表上摘下
        q = p->pre;                // 以下查找 p 结点的插入位置
        while (q != L && q->freq <= p->freq)
            q = q->pre;            // 向前找第一个频度更大的结点
        p->next = q->next;
        if (q->next != NULL) q->next->pre = p;  // 将 p 排在同频度结点的第一个
        p->pre = q;
        q->next = p;
    }
    return p;                      // 返回值为 x 的结点的指针
}
```

##### 补充说明

你的代码存在严重 bug：①内层 `while (q->pre != NULL)` 中，当 `q->freq != p->freq` 时循环体不执行任何操作，`q` 永不更新，直接死循环；②即使进入 `if` 分支，末尾 `q = p->next` 而 `p->next` 刚被赋值为 `q` 自身，同样死循环；③未先将 p 从链表摘下就改写 q 的指针，会破坏链表结构。正确做法是参考参考答案：先摘除 p，再向前找到第一个 `freq > p->freq` 的结点插在其后

##### 评分分析

**评分：2 / 10**

| 方面 | 得分 | 说明 |
|------|:----:|------|
| 查找结点 | ✅ 满分 | 正确遍历找到值为 x 的结点，未找到返回 NULL |
| 频度自增 | ✅ 满分 | `p->freq++` 正确 |
| 重排逻辑 | ❌ 扣 6 分 | 死循环（`q->freq != p->freq` 时 q 不更新）、未先摘除 p 就改写指针破坏链表、插入位置查找只处理 `==` 情况逻辑错误 |
| 边界处理 | ❌ 扣 2 分 | 未处理 p 为首结点或前驱频度更大的情况（此时无需移动） |

##### 修改建议

1. **先摘除再插入**：将 p 从链表中摘下（`p->pre->next = p->next` 并回接 `p->next->pre`），再找插入位置
2. **插入位置查找条件**：向前找第一个频度大于 p 的结点，即 `while (q != L && q->freq <= p->freq) q = q->pre;`，插在该结点之后，同频度时自然排在新访问结点之前
3. **避免死循环**：确保循环内 `q` 每次迭代都向前移动（`q = q->pre`），不要用 `q = p->next` 这类自我引用
