---
名称: Tec-2 指令详解
章: 05 计组课设
节: "[[第一章 Tec-2 基础入门]]"
tags:
  - 知识点
index: 1
---
##### E 指令：逐字输入

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/69bc0352b2744a63b1abb57c29ea2730.png)

最前面的 `0900` 提示当前位置，后面的 `0000` 提示当前位置的内容`Enter` 代表改动结束，`Space` 代表改下一个，每行显示 5 个字

##### A 指令：编译输入

E 输入机器语言，A 输入高级语言，两者作用相同

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/ccf12a50d5124ffc85b9d8f13a378495.png)

E 指令直接把 `AC00` 存入 `0800`；A 指令把 `RET` 编译成 machine code `AC00`，存入 `0801`

##### D 指令：显示内容

`D800` 会从 `0800` 位置向后显示 128 个字

##### U 指令：反编译

A 指令的逆，将 machine code 反编译为可读指令

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/b67bc40873b3427fbd90c3234e4f04b2.png)

##### G 指令：运行代码

运行至 `RET` 为止，`RET` 代表代码终止（相当于 return）没有 `RET` 会导致死循环

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/a04adb0d6a3e4b0fb51e81ae727b9a0a.png)

##### R 指令：显示寄存器

显示所有寄存器的值PC 存放当前指令
