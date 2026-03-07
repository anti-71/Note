---
名称: HDFS 的基础架构
章: 02 大数据
节: "[[第二章 Hadoop HDFS 分布式文件系统]]"
tags:
  - 知识点
index: 3
---
### 一、HDFS

HDFS 是 Hadoop 三大组件（HDFS、MapReduce、YARN）之一

- 全称是：Hadoop Distributed File System（Hadoop 分布式文件系统）
- 是 Hadoop 技术栈内提供的分布式数据存储解决方案
- 可以在多台服务器上构建存储集群，存储海量的数据

HDFS 是一个典型的 **主从模式** 架构

### 二、HDFS 的基础架构

HDFS 集群（分布式存储）

- 主角色：NameNode
- 从角色：DataNode
- 辅助角色：SecondaryNameNode

**NameNode**：

- HDFS 系统的主角色，是一个独立的进程
- 负责管理 HDFS 整个文件系统
- 负责管理 DataNode

**SecondaryNameNode**：

- NameNode 的辅助，是一个独立进程
- 主要帮助 NameNode 完成元数据整理工作（打杂）

**DataNode**：

- HDFS 系统的从角色，是一个独立进程
- 主要负责数据的存储，即存入数据和取出数据