---
名称: HDFS 数据的读写流程
章: 02 大数据
节: "[[第二章 Hadoop HDFS 分布式文件系统]]"
tags:
  - 知识点
index: 12
---
##### 数据写入流程

1. 客户端向 NameNode 发起请求
2. NameNode 审核权限、剩余空间后，满足条件允许写入，并告知客户端写入的 DataNode 地址
3. 客户端向指定的 DataNode 发送数据包
4. 被写入数据的 DataNode 同时完成数据副本的复制工作，将其接收的数据分发给其它 DataNode
5. 例如，DataNode1 复制给 DataNode2，然后基于 DataNode2 复制给 Datanode3 和 DataNode4
6. 写入完成客户端通知 NameNode，NameNode 做元数据记录工作

**关键信息点**：

- NameNode 不负责数据写入，只负责元数据记录和权限审批
- 客户端直接向 1 台 DataNode 写数据，这个 DataNode 一般是离客户端最近（网络距离）的那一个
- 数据块副本的复制工作，由 DataNode 之间自行完成（构建一个 PipLine，按顺序复制分发）

##### 数据读取流程

1. 客户端向 NameNode 申请读取某文件
2. NameNode 判断客户端权限等细节后，允许读取，并返回此文件的 block 列表
3. 客户端拿到 block 列表后自行寻找 DataNode 读取即可

**关键点**：

1. 数据同样不通过 NameNode 提供
2. NameNode 提供的 block 列表，会基于网络距离计算尽量提供离客户端最近的

   这是因为 1 个 block 有 3 份，会尽量找离客户端最近的那一份让其读取
