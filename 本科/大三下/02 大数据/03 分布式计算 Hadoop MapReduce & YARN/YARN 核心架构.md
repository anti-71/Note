---
名称: YARN 核心架构
章: 02 大数据
节: "[[第三章 分布式计算 Hadoop MapReduce & YARN]]"
tags:
  - 知识点
index: 4
---
##### YARN 架构

- HDFS，**主从架构**，有 2 个角色
  - 主（Master）角色：NameNode
  - 从（Slave）角色：DataNode
- YARN，**主从架构**，有 2 个角色
  - 主（Master）角色：**ResourceManager**
  - 从（Slave）角色：**NodeManager**

---

- **ResourceManager**：整个集群的资源调度者，负责协调调度各个程序所需的资源。
- **NodeManager**：单个服务器的资源调度者，负责调度单个服务器上的资源提供给应用程序使用

> [!question] 
> 如何实现服务器上精准分配如下的硬件资源呢？

开辟的空间，称之为：**容器**

##### YARN 容器

**容器**（Container）

- NodeManager **预先占用** 这一部分资源
- 然后将这一部分资源提供给程序使用
- 程序运行在容器（集装箱）内，无法突破容器的资源限制
