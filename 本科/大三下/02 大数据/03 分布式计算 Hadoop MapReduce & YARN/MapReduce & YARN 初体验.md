---
名称: MapReduce & YARN 初体验
章: 02 大数据
节: "[[第三章 分布式计算 Hadoop MapReduce & YARN]]"
tags:
  - 知识点
index: 6
---
### （一）集群启停命令

##### 一键启动脚本

**启动**：

`$HADOOP_HOME/ sbin/start-yarn.sh`

- 从 `yarn-site.xml` 中读取配置，确定 ResourceManager 所在机器，并启动它
- 读取 workers 文件，确定机器，启动全部的 NodeManager
- 在当前机器启动 ProxyServer（代理服务器）

**关闭**：

`$HADOOP_HOME/sbin/stop-yarn.sh`

##### 单进程启停

除了一键启停外，也可以单独控制进程的启停

- `$HADOOP_HOME/bin/yarn`，此程序也可以用以单独控制 **所在机器** 的进程的启停
    
    用法：`yarn --daemon (start|stop) (resourcemanager|nodemanager|proxyserver)`
    
- `$HADOOP_HOME/bin/mapred`，此程序也可以用以单独控制 **所在机器** 的 **历史服务器** 的启停
    
    用法：`mapred --daemon (start|stop) historyserver`

### （二）提交 MapReduce 命令到 YARN 执行