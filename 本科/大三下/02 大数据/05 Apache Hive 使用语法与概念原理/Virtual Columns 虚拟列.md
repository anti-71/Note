---
名称: Virtual Columns 虚拟列
章: 02 大数据
节: "[[第五章 Apache Hive 使用语法与概念原理]]"
tags:
  - 知识点
index: 15
---
##### Virtual Columns 虚拟列

**虚拟列** 是 Hive 内置的可以在查询语句中使用的特殊标记，可以查询数据本身的详细参数

Hive 目前可用 3 个虚拟列：

- INPUT__FILE__NAME，显示数据行所在的具体文件
- BLOCK__OFFSET__INSIDE__FILE，显示数据行所在文件的偏移量
- ROW__OFFSET__INSIDE__BLOCK，显示数据所在 HDFS 块的偏移量
    - 此虚拟列需要设置：SET hive.exec.rowoffset=true 才可使用

> [!example] 示例
> ```sql
> SELECT *, INPUT__FILE__NAME, BLOCK__OFFSET__INSIDE__FILE, ROW__OFFSET__INSIDE__BLOCK FROM itheima.course
> ```