---
名称: KMP 算法的进一步优化
章: 第四章 串
节: "[[第二节 串的模式匹配]]"
tags:
  - 知识点
index: 6
---
前面定义的 next 数组在某些情况下仍存在缺陷，还可进一步优化

> [!example] 模式串 'aaaab' 与主串 'aaabaaaab' 进行匹配
> 
> | 主串         | a   | a   | a   | b   | a   | a   | a   | a   | b   |     |
> | ---------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
> | 模式串        | a   | a   | a   | a   | b   |     |     |     |     |     |
> | j          | 1   | 2   | 3   | 4   | 5   |     |     |     |     |     |
> | next[j]    | 0   | 1   | 2   | 3   | 4   |     |     |     |     |     |
> | nextval[j] | 0   | 0   | 0   | 0   | 4   |     |     |     |     |     |
> 
> 当 i=4、j=4 时，主串字符 $s_4$ 与模式串字符 $p_4$（b ≠ a）失配
> 
> 若使用之前的 next 数组，则还需依次进行 $s_4$ 与 $p_3$、$s_4$ 与 $p_2$、$s_4$ 与 $p_1$ 的三次比较
> 
> 然而，由于 next[4] = 3 且 $p_3=p_4=a$，同理 next[3] = 2 且 $p_2=p_3=a$，next[2] = 1 且 $p_1=p_2=a$，由于这些回退位置上的字符均与 $p_4$ 相同，继续比较只会重复同样的失配过程，造成不必要的开销

问题根源在于：**不应出现** $p_j=p_{\text{next}[j]}$ 

理由是：当 $p_j \neq s_i$ 时，下一轮将用 $p_{\text{next}[j]}$ 与 $s_i$ 比较；若 $p_{\text{next}[j]}=p_j$，则相当于用与 $p_j$ 相同的字符去匹配已知不等的 $s_i$，结果必定仍是失配

那么，若出现 $p_j=p_{\text{next}[j]}$，应如何处理？ 

此时应不断将 next[j] 替换为 next[next[j]]，直到找到某个位置 k，使得 $p_j \neq p_k$ 或 k = 0 为止，修正后的新数组称为 **nextval 数组**

计算 nextval 数组的算法如下（匹配算法不变）

```c
void get_nextval(SString T, int nextval[]) {
    int i=1, j=0;
    nextval[1]=0;
    while(i<T.length) {
        if(j==0 || T.ch[i]==T.ch[j]) {
            ++i; ++j;
            if(T.ch[i]!=T.ch[j])  nextval[i]=j;
            else  nextval[i]=nextval[j];
        }
        else
            j=nextval[j];
    }
}
```

> [!note] 手算解题
> 先求 next 数组，再由 next 数组求 nextval 数组
> 
> 不断将 next[j] 替换为 next[next[j]]，直到找到某个位置两个字符不等或值为 0