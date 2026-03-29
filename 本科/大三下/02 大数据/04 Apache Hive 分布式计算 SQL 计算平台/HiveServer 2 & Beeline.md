---
名称: HiveServer 2 & Beeline
章: 02 大数据
节: "[[第四章 Apache Hive 分布式计算 SQL 计算平台]]"
tags:
  - 知识点
index: 6
---
##### HiveServer 2 服务

在启动 Hive 的时候，除了必备的 Metastore 服务外，我们前面提过有 2 种方式使用 Hive：

1. 方式 1

`bin/hive` 即 Hive 的 Shell 客户端，可以直接写 SQL

2. 方式 2

`bin/hive --service hiveserver2`

后台执行脚本：

```bash
nohup bin/hive --service hiveserver2 >> logs/hiveserver2.log 2>&1 &
```

`bin/hive --service metastore`，启动的是元数据管理服务

`bin/hive --service hiveserver2`，启动的是 HiveServer 2 服务

HiveServer 2 是 Hive 内置的一个 ThriftServer 服务，提供 Thrift 端口供其它客户端链接

可以连接 ThriftServer 的客户端有：

- Hive 内置的 beeline 客户端工具（命令行工具）
- 第三方的图形化 SQL 工具，如 DataGrip、DBeaver、Navicat 等

##### 启动

在 hive 安装的服务器上，**首先启动 metastore 服务，然后启动 hiveserver2 服务**

```bash
#先启动metastore服务 然后启动hiveserver2服务
nohup bin/hive --service metastore >> logs/metastore.log 2>&1 &
nohup bin/hive --service hiveserver2 >> logs/hiveserver2.log 2>&1 &
```

##### beeline

1. 在 node 1 上使用 beeline 客户端进行连接访问
   
   需要注意 **hiveserver2 服务启动之后需要稍等一会才可以对外提供服务**

2. Beeline 是 JDBC 的客户端，通过 JDBC 协议和 Hiveserver 2 服务进行通信，协议的地址是：
   
   `jdbc:hive2://node1:10000`
