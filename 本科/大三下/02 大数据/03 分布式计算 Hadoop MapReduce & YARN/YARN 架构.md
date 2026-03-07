---
名称: YARN 架构
章: 02 大数据
节: "[[第三章 分布式计算 Hadoop MapReduce & YARN]]"
tags:
  - 知识点
index: 4
---
### （一）核心架构

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

### （二）辅助架构

##### YARN 辅助角色

YARN 的架构中除了核心角色，即：

- **ResourceManager**：集群资源总管家
- **NodeManager**：单机资源管家

还可以搭配 2 个辅助角色使得 YARN 集群运行更加稳定：

- **代理服务器**（ProxyServer）：Web Application Proxy Web 应用程序代理
- **历史服务器**（JobHistoryServer）：应用程序历史信息记录服务

##### Web 应用代理（Web Application Proxy）

代理服务器，即 Web 应用代理是 YARN 的一部分

默认情况下，它将作为资源管理器（RM）的一部分运行，但是可以配置为在 **独立模式** 下运行

> [!question] 
> 为什么要使用代理服务器？

为了减少通过 YARN 进行基于网络的攻击的可能性

YARN 在运行时会提供一个 WEB UI 站点（同 HDFS 的 WEB UI 站点一样）可供用户在浏览器内查看 YARN 的运行信息

对外提供 WEB 站点会有安全性问题，而代理服务器的功能就是最大限度保障对 WEB UI 的访问是安全的

比如：

- 警告用户正在访问一个不受信任的站点
- 剥离用户访问的 Cookie 等

开启代理服务器，可以提高 YARN 在开放网络中的安全性（但不是绝对安全，只能是辅助提高一些）

---

代理服务器默认集成在了 ResourceManager 中，也可以将其分离出来单独启动，如果要分离代理服务器：

1. 在 `yarn-site.xml` 中配置 `yarn.web-proxy.address` 参数即可（部署环节会使用到）

2. 并通过命令启动它即可

   ```bash
   $HADOOP_YARN_HOME/sbin/yarn-daemon.sh start proxyserver
   ```

##### JobHistoryServer 历史服务器

历史服务器的功能很简单：记录历史运行的程序的信息以及产生的日志并提供 WEB UI 站点供用户使用浏览器查看

> [!question] 
> 程序看日志不是日常操作吗？为何需要一个单独的历史服务器？

回答这个问题要从 YARN 的运行机制说起

运行日志，产生在容器中，太零散了，难以查看

而历史服务器能够统一收集到 HDFS，由历史服务器托管为 WEB UI 供用户在浏览器统一查看

---

JobHistoryServer 历史服务器 **功能**：

- 提供 WEB UI 站点，供用户在浏览器上查看程序日志
- 可以保留历史数据，随时查看历史运行程序信息

JobHistoryServer 需要 **配置**：

- 开启日志聚合，即从容器中抓取日志到 HDFS 集中存储
- 配置历史服务器端口和主机
