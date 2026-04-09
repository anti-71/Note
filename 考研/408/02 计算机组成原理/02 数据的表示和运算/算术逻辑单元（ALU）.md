---
名称: 算术逻辑单元（ALU）
章: 第二章 数据的表示和运算
节: "[[第二节 运算方法和运算电路]]"
tags:
  - 知识点
index: 5
---
##### ALU 的基本功能

ALU 是一种功能较强的组合逻辑电路，能够执行多种算术与逻辑运算

其中，加法和减法由 **带标志加法器** 直接完成

乘法和除法则通常通过 ALU 配合控制逻辑，以多次加减和移位的方式迭代实现

此外，ALU 还能执行与、或、非等基本逻辑运算

##### ALU 的基本结构

其基本结构如下图所示

![image.png|334x229](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/20260409180902423.png)

A 和 B 为两个 n 位操作数输入端，Cin​ 为进位输入端，ALUop 为操作控制信号，用于选择 ALU 执行的具体功能

ALU运算数、运算结果位数与计算机的机器字长相同

> [!example] 
> 当 ALUop 选择加法（Add）时，ALU 输出 A + B + Cin​

ALUop 的位数决定了可支持的操作种类数量

若支持 k 种功能，ALUop 位数 $\geqslant\lceil \log_{2}k\rceil$

标志信息送入 **PSW 程序状态寄存器**（有的也称标志寄存器 FR）

> [!example] 
> 下图展示了一位 ALU 的结构，可完成 “与”“或”“加法” 三种操作
> 
> ![image.png|349x255](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/20260409181051437.png)
> 
> 其中，加法由一个全加器实现，逻辑运算由专用门电路并行计算，最终通过多路选择器（MUX）根据 ALUop 选择输出结果
> 
> 由于有 3 种操作，ALUop 至少需要 2 位