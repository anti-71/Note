---
名称: 模拟实现 Hive 功能
章: 02 大数据
节: "[[第四章 Apache Hive 分布式计算 SQL 计算平台]]"
tags:
  - 知识点
index: 2
---

> [!question] 
> 如果让您设计 Hive 这款软件，要求能够实现：
> 
> - 用户只编写 sql 语句
> - Hive 自动将 **sql** 转换 **MapReduce** 程序并提交运行
> - 处理位于 HDFS 上的结构化数据
>    
> 如何实现？

##### 元数据管理

假设有如下结构化文本数据存储在 HDFS 中：

```
1,zhangsan,18,beijing
2,lisi,25,shanghai
3,allen,30,shanghai
4,wangwu,15,nanjing
5,james,45,hangzhou
6,tony,26,beijing
```

假设要执行：`SELECT city, COUNT(*) FROM t_user GROUP BY city;`

> [!question] 
> 对这个 SQL 翻译成 MapReduce 程序，会出现哪些困难？

- 数据文件在哪里？
- 使用什么符号作为列的分隔符？
- 哪些列可以作为 city 使用？
- city 列是什么类型的数据？

这些信息同时也需要有地方存储起来，方便多次使用

> [!question] 
> 如何存储最好呢？

很简单，找一个数据库即可，比如 **MySQL**

所以，我们可以总结出来第一个点，即构建分布式 SQL 计算，需要拥有：

- 元数据管理功能，即：
    - 数据位置
    - 数据结构
    - 等对数据进行描述
        进行记录

##### 解析器

解决了元数据管理后，我们还有一个至关重要的步骤，即完成 SQL 到 MapReduce 转换的功能

我们称呼它为：**SQL 解析器**，期待它能做到：

- SQL 分析
- SQL 到 MapReduce 程序的转换
- 提交 MapReduce 程序运行并收集执行结果

##### 基础架构

所以，当解析器也拥有了之后，我们就完成了一款基于 MapReduce 的，分布式 SQL 执行引擎的基础构建

即，核心组件需要有：

- 元数据管理，帮助记录各类元数据
- SQL 解析器，完成 SQL 到 MapReduce 程序的转换
    当拥有这 2 个组件，基本上分布式 SQL 计算的能力就实现了

##### Hive 架构

Apache Hive 其 2 大主要组件就是：**SQL 解析器** 以及 **元数据存储**