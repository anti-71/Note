---
名称: Apache Hadoop 概述
章: 02 大数据
节: "[[第一章 Hello 大数据分布式]]"
tags:
  - 知识点
index: 5
---
##### 什么是 Hadoop

Hadoop 是 Apache 软件基金会下的顶级开源项目，用以提供：

- 分布式数据存储
- 分布式数据计算
- 分布式资源调度

为一体的整体解决方案

ApacheHadoop 是典型的分布式软件框架，可以部署在 1 台乃至成干上万台服务器节点上协同工作

个人或企业可以借助 Hadoop 构建大规模服务器集群，完成海量数据的存储和计算

##### 为什么学习 Hadoop

近 10 年来，大数据技术体系一词一直和 Hadoop 是划上等号的，提起大数据技术基本就是在提及 Hadoop

随着近些年的发展，越来越多的新技术框架的出现，给大数据技术体系带来了丰富的生态，但是拥有元老地位的 Hadoop 依旧非常重要

为什么学习 Hadoop 有如下几个至关重要的原因：

- Hadoop 是最早的一批大数据技术框架，在市面上拥有极高的占有率和庞大的用户群体
- Hadoop 在大数据体系内，技术难度相对较低，非常适合作为大数据学习的入门技术栈

**所以，学习 Hadoop 不仅仅因为其适合入门，同时也可以为大数据学习打下良好的基础**

##### Hadoop 的功能

通常意义上，Hadoop 是一个整体，其内部还会细分为三个功能组件，分别是：

- HDFS 组件：HDFS 是 Hadoop 内的分布式存储组件
  - 可以构建分布式文件系统用于数据存储
- MapReduce 组件：MapReduce 是 Hadoop 内分布式计算组件
  - 提供编程接口供用户开发分布式计算程序
- YARN 组件：YARN 是 Hadoop 内分布式资源调度组件
  - 可供用户整体调度大规模集群的资源使用

##### Hadoop 发展

- Hadoop 创始人：**Doug Cutting**
- Hadoop 起源于 Apache Lucene 子项目：Nutch
  
  Nutch 的设计目标是构建一个大型的全网搜索引擎
  
  遇到瓶颈：如何解决数十亿网页的存储和索引问题
  
- **Google 三篇论文**
	- 《The Google file system》：谷歌分布式文件系统 GFS
	- 《MapReduce：Simplified Data Processing on Large Clusters》：谷歌分布式计算框架 MapReduce
	- 《Bigtable：A Distributed Storage System for Structured Data》：谷歌结构化数据存储系统

##### Hadoop 发行版本

Apache 开源社区版本：<http://hadoop.apache.org/>

商业发行版本：

- CDH（Cloudera's Distribution, including Apache Hadoop），Cloudera 公司出品，目前使用最多的商业版
- HDP（HortonworksDataPlatform），Hortonworks 公司出品，目前被 Cloudera 收购
- 星环，国产商业版，星环公司出品，在国内政企使用较多