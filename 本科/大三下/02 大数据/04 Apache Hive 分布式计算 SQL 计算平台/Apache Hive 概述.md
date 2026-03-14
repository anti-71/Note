---
名称: Apache Hive 概述
章: 02 大数据
节: "[[第四章 Apache Hive 分布式计算 SQL 计算平台]]"
tags:
  - 知识点
index: 1
---
##### 分布式 SQL 计算

对数据进行统计分析，**SQL** 是目前最为方便的编程工具

大数据体系中充斥着非常多的统计分析场景，所以，使用 SQL 去处理数据，在大数据中也是有极大的需求的

但是，MapReduce 支持程序开发（Java、Python 等），但不支持 SQL 开发

##### 分布式 SQL 计算 - Hive

Apache Hive 是一款分布式 SQL 计算的工具，其主要功能是：

- 将 **SQL 语句** 翻译成 **MapReduce 程序** 运行
    基于 Hive 为用户提供了分布式 SQL 计算的能力，写的是 SQL、执行的是 MapReduce

##### 为什么使用 Hive

使用 Hadoop MapReduce 直接处理数据所面临的问题

- 人员学习成本太高，需要掌握 java、Python 等编程语言
- MapReduce 实现复杂查询逻辑开发难度太大

使用 Hive 处理数据的好处

- 操作接口采用 **类 SQL 语法**，提供快速开发的能力（简单、容易上手）
- 底层执行 MapReduce，**可以完成分布式海量数据的 SQL 处理**