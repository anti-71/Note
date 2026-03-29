---
名称: MapReduce & YARN 的部署
章: 02 大数据
节: "[[第三章 分布式计算 Hadoop MapReduce & YARN]]"
tags:
  - 知识点
index: 6
---
##### 部署说明

- Hadoop HDFS 分布式文件系统，我们会启动：
    
    - **NameNode** 进程作为管理节点
    - **DataNode** 进程作为工作节点
    - **SecondaryNamenode** 作为辅助

- 同理，Hadoop YARN 分布式资源调度，会启动：
    
    - **ResourceManager** 进程作为管理节点
    - **NodeManager** 进程作为工作节点
    - **ProxyServer**、**JobHistoryServer** 这两个辅助节点
    
> [!question] 
> 那么，MapReduce 呢？

**MapReduce 运行在 YARN 容器内，无需启动独立进程**

所以关于 MapReduce 和 YARN 的部署，其实就是 2 件事情：

- 关于 MapReduce：修改相关配置文件，但是 **没有进程可以启动**
- 关于 YARN：修改相关配置文件，并启动 **ResourceManager**、**NodeManager** 进程以及辅助进程（代理服务器、历史服务器）

##### Hadoop 组件信息

| 组件               | 配置文件 | 启动进程                                                                                                                  | 备注      |
| ---------------- | ---- | --------------------------------------------------------------------------------------------------------------------- | ------- |
| Hadoop HDFS      | 需修改  | 需启动<br>・NameNode 作为主节点<br>・DataNode 作为从节点<br>・SecondaryNameNode 主节点辅助                                                 | 分布式文件系统 |
| Hadoop YARN      | 需修改  | 需启动<br>・ResourceManager 作为集群资源管理者<br>・NodeManager 作为单机资源管理者<br>・ProxyServer 代理服务器提供安全性<br>・JobHistoryServer 记录历史信息和日志 | 分布式资源调度 |
| Hadoop MapReduce | 需修改  | **无需启动任何进程**<br>MapReduce 程序运行在 YARN 容器内                                                                              | 分布式数据计算 |

##### 集群规划

有 3 台服务器，其中 node 1 配置较高

集群规划如下：

| 主机     | 角色                                                                |
| ------ | ----------------------------------------------------------------- |
| node 1 | ResourceManager<br>NodeManager<br>ProxyServer<br>JobHistoryServer |
| node 2 | NodeManager                                                       |
| node 3 | NodeManager                                                       |

##### MapReduce 配置文件

在 `$HADOOP_HOME/etc/hadoop` 文件夹内，修改：

- `mapred-env.sh` 文件，添加如下环境变量

```bash
# 设置JDK路径
export JAVA_HOME=/export/server/jdk
# 设置JobHistoryServer进程内存为1G
export HADOOP_JOB_HISTORYSERVER_HEAPSIZE=1000
# 设置日志级别为INFO
export HADOOP_MAPRED_ROOT_LOGGER=INFO,RFA
```

- `mapred-site.xml` 文件，添加如下配置信息

```xml
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
  <description>MapReduce的运行框架设置为YARN</description>
</property>

<property>
  <name>mapreduce.jobhistory.address</name>
  <value>node1:10020</value>
  <description>历史服务器通讯端口为node1:10020</description>
</property>

<property>
  <name>mapreduce.jobhistory.webapp.address</name>
  <value>node1:19888</value>
  <description>历史服务器web端口为node1的19888</description>
</property>

<property>
  <name>mapreduce.jobhistory.intermediate-done-dir</name>
  <value>/data/mr-history/tmp</value>
  <description>历史信息在HDFS的记录临时路径</description>
</property>

<property>
  <name>mapreduce.jobhistory.done-dir</name>
  <value>/data/mr-history/done</value>
  <description>历史信息在HDFS的记录路径</description>
</property>

<property>
  <name>yarn.app.mapreduce.am.env</name>
  <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
  <description>MapReduce HOME 设置为HADOOP_HOME</description>
</property>

<property>
  <name>mapreduce.map.env</name>
  <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
  <description>MapReduce HOME 设置为HADOOP_HOME</description>
</property>

<property>
  <name>mapreduce.reduce.env</name>
  <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
  <description>MapReduce HOME 设置为HADOOP_HOME</description>
</property>
```

##### YARN 配置文件

在 `$HADOOP_HOME/etc/hadoop` 文件夹内，修改：

- `yarn-env.sh` 文件，添加如下 4 行环境变量内容：

```bash
# 设置JDK路径的环境变量
export JAVA_HOME=/export/server/jdk
# 设置HADOOP_HOME的环境变量
export HADOOP_HOME=/export/server/hadoop
# 设置配置文件路径的环境变量
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
# 设置日志文件路径的环境变量
export HADOOP_LOG_DIR=$HADOOP_HOME/logs
```

- `yarn-site.xml` 文件，配置如下

```xml
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>node1</value>
  <description>ResourceManager设置在node1节点</description>
</property>

<property>
  <name>yarn.nodemanager.local-dirs</name>
  <value>/data/nm-local</value>
  <description>NodeManager中间数据本地存储路径</description>
</property>

<property>
  <name>yarn.nodemanager.log-dirs</name>
  <value>/data/nm-log</value>
  <description>NodeManager数据日志本地存储路径</description>
</property>

<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
  <description>为MapReduce程序开启Shuffle服务</description>
</property>

<property>
  <name>yarn.log.server.url</name>
  <value>http://node1:19888/jobhistory/logs</value>
  <description>历史服务器URL</description>
</property>

<property>
  <name>yarn.web-proxy.address</name>
  <value>node1:8089</value>
  <description>代理服务器主机和端口</description>
</property>

<property>
  <name>yarn.log-aggregation-enable</name>
  <value>true</value>
  <description>开启日志聚合</description>
</property>

<property>
  <name>yarn.nodemanager.remote-app-log-dir</name>
  <value>/tmp/logs</value>
  <description>程序日志HDFS的存储路径</description>
</property>

<property>
  <name>yarn.resourcemanager.scheduler.class</name>
  <value>org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler</value>
  <description>选择公平调度器</description>
</property>
```

##### 分发配置文件

MapReduce 和 YARN 的配置文件修改好后，需要分发到其它的服务器节点中

```bash
scp mapred-env.sh mapred-site.xml yarn-env.sh yarn-site.xml node2:`pwd`/
scp mapred-env.sh mapred-site.xml yarn-env.sh yarn-site.xml node3:`pwd`/
```

分发完成配置文件，就可以启动 YARN 的相关进程啦

##### 集群启动命令介绍

常用的进程启动命令如下：

- 一键启动 YARN 集群：`$HADOOP_HOME/sbin/start-yarn.sh`
    
    - 会基于 `yarn-site.xml` 中配置的 `yarn.resourcemanager.hostname` 来决定在哪台机器上启动 `resourcemanager`
    - 会基于 `workers` 文件配置的主机启动 `NodeManager`
    
- 一键停止 YARN 集群：`$HADOOP_HOME/sbin/stop-yarn.sh`
- 在当前机器，单独启动或停止进程
    
    - `$HADOOP_HOME/bin/yarn --daemon start|stop resourcemanager|nodemanager|proxyserver`
    - `start` 和 `stop` 决定启动和停止
    - 可控制 `resourcemanager`、`nodemanager`、`proxyserver` 三种进程
    
- 历史服务器启动和停止
    
    - `$HADOOP_HOME/bin/mapred --daemon start|stop historyserver`
    

##### 查看 YARN 的 WEB UI 页面

- 打开 `http://node1:8088` 即可看到 YARN 集群的监控页面（ResourceManager 的 WEB UI）

##### 提交求圆周率示例程序

可以执行如下命令，使用蒙特卡罗算法模拟计算求 PI（圆周率）

```bash
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar pi 3 1000
```

- 参数 pi 表示要运行的 Java 类，这里表示运行 jar 包中的求 pi 程序
- 参数 3，表示设置几个 map 任务
- 参数 1000，表示模拟求 PI 的样本数（越大求的 PI 越准确，但是速度越慢）