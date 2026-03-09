---
名称: MapReduce & YARN 的部署
章: 02 大数据
节: "[[第三章 分布式计算 Hadoop MapReduce & YARN]]"
tags:
  - 知识点
index: 5
---
# 部署说明

- Hadoop HDFS 分布式文件系统，我们会启动：
    
    - **NameNode** 进程作为管理节点
    - **DataNode** 进程作为工作节点
    - **SecondaryNamenode** 作为辅助

- 同理，Hadoop YARN 分布式资源调度，会启动：
    
    - **ResourceManager** 进程作为管理节点
    - **NodeManager** 进程作为工作节点
    - **ProxyServer**、**JobHistoryServer** 这两个辅助节点
    
- 那么，MapReduce 呢？

**MapReduce 运行在 YARN 容器内，无需启动独立进程**

所以关于 MapReduce 和 YARN 的部署，其实就是 2 件事情：

- 关于 MapReduce：修改相关配置文件，但是 **没有进程可以启动**
- 关于 YARN：修改相关配置文件，并启动 **ResourceManager**、**NodeManager** 进程以及辅助进程（代理服务器、历史服务器）