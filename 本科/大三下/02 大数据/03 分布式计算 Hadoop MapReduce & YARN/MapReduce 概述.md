---
名称: MapReduce 概述
章: 02 大数据
节: "[[第三章 分布式计算 Hadoop MapReduce & YARN]]"
tags:
  - 知识点
index: 2
---
##### 分布式计算框架 - MapReduce

MapReduce 即 Hadoop 内提供的进行分布式计算的组件，是 “分散 -> 汇总” 模式的分布式计算框架，可供开发人员开发相关程序进行分布式数据计算

MapReduce 提供了 2 个编程接口：

- Map
- Reduce

其中：

- Map 功能接口提供了 “**分散**” 的功能，由服务器分布式对数据进行处理
- Reduce 功能接口提供了 “**汇总（聚合）**” 的功能，将分布式的处理结果汇总统计

用户如需使用 MapReduce 框架完成自定义需求的程序开发，只需要使用 Java、Python 等编程语言，实现 Map Reduce 功能接口即可

##### MapReduce 执行原理

现在，我们借助一个案例，简单分析一下，MapReduce 是如何完成分布式计算的

假设有一个文件，内部记录了许多的单词

且已经开发好了一个 MapReduce 程序，功能是统计每个单词出现的次数

假定有 4 台服务器用以执行 MapReduce 任务：

- 可以 3 台服务器执行 Map（统计各自部分文件的单词数量），1 台服务器执行 Reduce（汇总每个 Map 提交的结果）
- 将任务分解为：3 个 Map (分散) Task，1 个 Reduce (汇总) Task
