---
名称: HDFS 集群环境部署
章: 02 大数据
节: "[[第二章 Hadoop HDFS 分布式文件系统]]"
tags:
  - 知识点
index: 4
---
### 一、安装包下载

官方网址：<https://hadoop.apache.org>

清华大学开源软件镜像站：[Index of /apache/hadoop/common](https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/)

该文档使用当前最新的发行版：3.4.2 版

### 二、集群规划

| 节点  |                 服务                  |
| :---: | :-----------------------------------: |
| node1 | NameNode、DataNode、SecondaryNameNode |
| node2 |               DataNode                |
| node3 |               DataNode                |

### 三、上传 ＆ 解压

> [!failure] 注意
> 请确认已经完成前置准备中的服务器创建、固定 IP、防火墙关闭、Hadoop 用户创建、SSH 免密、JDK 部署等操作

以下操作 node1 节点执行以 root 身份登陆

1. 上传 Hadoop 安装包到 node1 节点中

2. 解压缩安装包到 `/export/server/` 中

   ```bash
tar -zxvf /home/hadoop/hadoop-3.4.2.tar.gz -C /export/server
   ```

3. 构建软链接

   ```bash
cd /export/server
ln -s /export/server/hadoop-3.4.2 hadoop
   ```

4. 进入 hadoop 安装包内

   ```bash
cd hadoop
   ```

### 四、Hadoop 安装包目录结构

cd 进入 Hadoop 安装包内，通过 `ls -l` 命令查看文件夹内部结构

各个文件夹含义如下

- **bin，存放 Hadoop 的各类程序（命令）**
- etc，存放 Hadoop 的配置文件
- include，C 语言的一些头文件
- lib，存放 Linux 系统的动态链接库（.so 文件）
- libexec，存放配置 Hadoop 系统的脚本文件（.sh 和.cmd）
- licenses-binary，存放许可证文件
- **sbin，管理员程序（super bin）**
- share，存放二进制源码（Java jar 包）

### 五、修改配置文件，应用自定义设置

配置 HDFS 集群，我们主要涉及到如下文件的修改：

- workers：配置从节点（DataNode）有哪些
- hadoop-env.sh：配置 Hadoop 的相关环境变量
- core-site.xml：Hadoop 核心配置文件
- hdfs-site.xml：HDFS 核心配置文件

这些文件均存在与$HADOOP_HOME/etc/hadoop 文件夹中

> [!note]
> $HADOOP_HOME 是后续我们要设置的环境变量，其指代 Hadoop 安装文件夹，即/export/server/hadoop

##### 配置 workers 文件

```bash
# 进入配置文件目录
cd etc/hadoop
# 编辑 workers 文件
vim workers
# 填入如下内容
node1
node2
node3
```

填入的 node1、node2、node3 表明集群记录了三个从节点（DataNode）

##### 配置 hadoop-env.sh 文件

```bash
# 填入如下内容
export JAVA_HOME=/export/server/jdk
export HADOOP_HOME=/export/server/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_LOG_DIR=$HADOOP_HOME/logs
```

- JAVA_HOME，指明 JDK 环境的位置在哪
- HADOOP_HOME，指明 Hadoop 安装位置
- HADOOP_CONF_DIR，指明 Hadoop 配置文件目录位置
- HADOOP_LOG_DIR，指明 Hadoop 运行日志目录位置

通过记录这些环境变量，来指明上述运行时的重要信息

##### 配置 core-site.xml 文件

在文件内部填入如下内容

```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://node1:8020</value>
  </property>

  <property>
    <name>io.file.buffer.size</name>
    <value>131072</value>
  </property>
</configuration>
```

- key：fs.defaultFS
- 含义：HDFS 文件系统的网络通讯路径
- 值：hdfs://node1:8020
  - 协议为 hdfs://
  - namenode 为 node1
  - namenode 通讯端口为 8020
- key：io.file.buffer.size
- 含义：io 操作文件缓冲区大小
- 值：131072 bit

---

- hdfs://node1:8020 为整个 HDFS 内部的通讯地址，应用协议为 hdfs://（Hadoop 内置协议）
- 表明 DataNode 将和 node1 的 8020 端口通讯，node1 是 NameNode 所在机器
- 此配置固定了 node1 必须启动 NameNode 进程

##### 配置 hdfs-site.xml 文件

```xml
# 在文件内部填入如下内容
<configuration>
  <property>
    <name>dfs.datanode.data.dir.perm</name>
    <value>700</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/data/nn</value>
  </property>
  <property>
    <name>dfs.namenode.hosts</name>
    <value>node1,node2,node3</value>
  </property>
  <property>
    <name>dfs.blocksize</name>
    <value>268435456</value>
  </property>
  <property>
    <name>dfs.namenode.handler.count</name>
    <value>100</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>/data/dn</value>
  </property>
</configuration>
```

针对 hdfs-site.xml，简单分析一下配置文件的内容：

- key：dfs.datanode.data.dir.perm
- 含义：hdfs 文件系统，默认创建的文件权限设置
- 值：700，即：rwx------

---

- key：dfs.namenode.name.dir
- 含义：NameNode 元数据的存储位置
- 值：/data/nn，在 node1 节点的 /data/nn 目录下

---

- key：dfs.namenode.hosts
- 含义：NameNode 允许哪几个节点的 DataNode 连接（即允许加入集群）
- 值：node1、node2、node3，这三台服务器被授权

---

- key：dfs.blocksize
- 含义：hdfs 默认块大小
- 值：268435456（256MB）

---

- key：dfs.namenode.handler.count
- 含义：namenode 处理的并发线程数
- 值：100，以 100 个并行度处理文件系统的管理任务

---

- key：dfs.datanode.data.dir
- 含义：从节点 DataNode 的数据存储目录
- 值：/data/dn，即数据存放在 node1、node2、node3，三台机器的 /data/dn 内

### 六、准备数据目录

根据下述 2 个配置项：

```xml
<property>
  <name>dfs.namenode.name.dir</name>
  <value>/data/nn</value>
</property>
```

```xml
<property>
  <name>dfs.datanode.data.dir</name>
  <value>/data/dn</value>
</property>
```

- namenode 数据存放 node1 的 /data/nn
- datanode 数据存放 node1、node2、node3 的 /data/dn

所以应该

- 在 node1 节点：
  - `mkdir -p /data/nn`
  - `mkdir -p /data/dn`
- 在 node2 和 node3 节点：
  - `mkdir -p /data/dn`

### 七、分发 Hadoop 文件夹

目前，已经基本完成 Hadoop 的配置操作，可以从 node1 将 hadoop 安装文件夹远程复制到 node2、node3

- 分发

```bash
# 在 node1 执行如下命令
cd /export/server
scp -r hadoop-3.4.2 node2:`pwd`/
scp -r hadoop-3.4.2 node3:`pwd`/
```

- **在 node2 执行**，为 hadoop 配置软链接

```bash
# 在 node2 执行如下命令
ln -s /export/server/hadoop-3.4.2 /export/server/hadoop
```

- **在 node3 执行**，为 hadoop 配置软链接

```bash
# 在 node3 执行如下命令
ln -s /export/server/hadoop-3.4.2 /export/server/hadoop
```

### 八、配置环境变量

为了方便我们操作 Hadoop，可以将 Hadoop 的一些脚本、程序配置到 PATH 中，方便后续使用

在 Hadoop 文件夹中的 bin、sbin 两个文件夹内有许多的脚本和程序，现在来配置一下环境变量

1. `vim /etc/profile`

     ```bash
     #在/etc/profile 文件底部追加如下内容
     export HADOOP_HOME=/export/server/hadoop
     export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
     ```

2. **在 node2 和 node3 配置同样的环境变量**

### 九、授权为 hadoop 用户

hadoop 部署的准备工作基本完成

为了确保安全，hadoop 系统不以 root 用户启动，我们 **以普通用户 hadoop 来启动整个 Hadoop 服务**

所以，现在需要对文件权限进行授权

> [!failure]
> 请确保已经提前创建好了 hadoop 用户（前置准备章节中有讲述），并配置好了 hadoop 用户之间的免密登录

- 以 **root 身份**，在 **node1、node2、node3 三台服务器** 上均执行如下命令

```bash
# 以 root 身份，在三台服务器上均执行
chown -R hadoop:hadoop /data
chown -R hadoop:hadoop /export
```

### 十、格式化整个文件系统

前期准备全部完成，现在对整个文件系统执行初始化

- 格式化 namenode

```bash
# 确保以 hadoop 用户执行
su - hadoop
# 格式化 namenode
hadoop namenode -format
```

- 启动

```bash
# 一键启动 hdfs 集群
start-dfs.sh
# 一键关闭 hdfs 集群
stop-dfs.sh

# 如果遇到命令未找到的错误，表明环境变量未配置好，可以以绝对路径执行
/export/server/hadoop/sbin/start-dfs.sh
/export/server/hadoop/sbin/stop-dfs.sh
```