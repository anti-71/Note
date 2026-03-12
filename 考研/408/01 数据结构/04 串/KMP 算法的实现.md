---
名称: KMP 算法的实现
章: 第四章 串
节: "[[第二节 串的模式匹配]]"
tags:
  - 知识点
index: 5
---
##### next 数组求解算法

通过上述分析，可写出求解 next 数组的程序如下：

```c
void get_next(SString T, int next[]) {
    int i=1, j=0;
    next[1]=0;
    while(i<T.length) {
        if(j==0 || T.ch[i]==T.ch[j]) {
            ++i; ++j;
            next[i]=j;  //若 p_i=p_j，则 next[j+1]=next[j]+1
        }
        else
            j=next[j];  //否则令 j=next[j]，循环继续
    }
}
```

计算机上执行的效率很高，但手工计算时，仍需采用前文介绍的直观方法

##### KMP 算法代码

与 next 数组的求解相比，KMP 算法的匹配过程相对简洁，其结构与简单模式匹配极为相似

**关键区别**：

当发生失配时，主串指针 i 保持不变，仅将模式串指针 j 回退至 next[j] 所指示的位置，并继续比较；特别地，当 j = 0 时，将 i 和 j 同时加 1

也就是说，若模式串首字符失配，则将模式串向右滑动一位，下一轮匹配从主串第 i + 1 个位置开始

**具体实现**：

```c
int Index_KMP(SString S, SString T, int next[]) {
    int i=1, j=1;
    while(i<=S.length && j<=T.length) {
        if(j==0 || S.ch[i]==T.ch[j]) {
            ++i; ++j;          //继续比较后继字符
        }
        else
            j=next[j];         //模式串向右滑动
    }
    if(j>T.length)
        return i-T.length;    //匹配成功
    else
        return 0;
}
```

尽管简单模式匹配的最坏时间复杂度为 $O(mn)$，而 KMP 算法可达到 $O(m+n)$，但在一般情况下，简单模式匹配的平均执行时间往往接近 $O(m+n)$，因此至今仍被采用

KMP 算法仅在主串与模式串存在大量 “部分匹配” 时才显著优于暴力匹配，其 **核心优点** 在于主串指针不回溯