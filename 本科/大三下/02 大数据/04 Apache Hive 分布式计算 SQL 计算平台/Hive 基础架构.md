---
名称: Hive 基础架构
章: 02 大数据
节: "[[第四章 Apache Hive 分布式计算 SQL 计算平台]]"
tags:
  - 知识点
index: 3
---
##### Hive 组件

1. 元数据存储

通常是存储在关系数据库如 mysql/derby 中

Hive 中的元数据包括表的名字，表的列和分区及其属性，表的属性（是否为外部表等），表的数据所在目录等

——Hive 提供了 Metastore 服务进程提供元数据管理功能

2. Driver 驱动程序

包括语法解析器、计划编译器、优化器、执行器

完成 HQL 查询语句从词法分析、语法分析、编译、优化以及查询计划的生成

生成的查询计划存储在 HDFS 中，并在随后有执行引擎调用执行

这部分内容不是具体的服务进程，而是封装在 Hive 所依赖的 Jar 文件即 Java 代码中

3. 用户接口

包括 CLI、JDBC/ODBC、WebGUI

其中，CLI (command line interface) 为 shell 命令行；Hive 中的 Thrift 服务器允许外部客户端通过网络与 Hive 进行交互，类似于 JDBC 或 ODBC 协议

WebGUI 是通过浏览器访问 Hive

——Hive 提供了 Hive Shell、ThriftServer 等服务进程向用户提供操作接口