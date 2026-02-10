# 第二章 Hadoop HDFS 分布式文件系统

## 第一节 为什么需要分布式存储

靠数量取胜，多台服务器组合，才能 Hold 住

分布式不仅仅是解决了能存的问题，多台服务器协同工作带来的也是性能的横向扩展

- 三倍的网络传输效率
- 三倍的磁盘写入效率

## 第二节 分布式的基础架构分析

### 一、分布式的基础架构

**数量多**，在现实生活中往往带来的不是提升，而是：混乱

大数据体系中，分布式的调度主要有 2 类架构模式：

- 去中心化模式
- 中心化模式

**去中心化模式**，没有明确的中心

众多服务器之间基于 **特定规则** 进行同步协调

### 二、主从模式

大数据框架，大多数的基础架构上，都是符合：**中心化模式的**

即：有一个中心节点（服务器）来统筹其它服务器的工作，统一指挥，统一调派，避免混乱

这种模式，也被称之为：**一主多从模式**，**简称主从模式**（Master And Slaves）

主从模式（中心化模式）在现实生活中同样很常见：

- 公司企业管理
- 组织管理
- 行政管理
- 等等

我们学习的 Hadoop 框架，就是一个典型的主从模式（中心化模式）架构的技术框架

## 第三节 HDFS 的基础架构

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

## 第四节 HDFS 集群环境部署

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

> $HADOOP_HOME 是后续我们要设置的环境变量，其指代 Hadoop 安装文件夹，即/export/server/hadoop

##### 配置 workers 文件

```bash
# 进入配置文件目录
cd etc/hadoop
# 编辑workers文件
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
# 在node1执行如下命令
cd /export/server
scp -r hadoop-3.4.2 node2:`pwd`/
scp -r hadoop-3.4.2 node3:`pwd`/
```

- **在 node2 执行**，为 hadoop 配置软链接

```bash
# 在node2执行如下命令
ln -s /export/server/hadoop-3.4.2 /export/server/hadoop
```

- **在 node3 执行**，为 hadoop 配置软链接

```bash
# 在node3执行如下命令
ln -s /export/server/hadoop-3.4.2 /export/server/hadoop
```

### 八、配置环境变量

为了方便我们操作 Hadoop，可以将 Hadoop 的一些脚本、程序配置到 PATH 中，方便后续使用

在 Hadoop 文件夹中的 bin、sbin 两个文件夹内有许多的脚本和程序，现在来配置一下环境变量

1. `vim /etc/profile`

     ```bash
     #在/etc/profile文件底部追加如下内容
     export HADOOP_HOME=/export/server/hadoop
     export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
     ```

2. **在 node2 和 node3 配置同样的环境变量**

### 九、授权为 hadoop 用户

hadoop 部署的准备工作基本完成

为了确保安全，hadoop 系统不以 root 用户启动，我们 **以普通用户 hadoop 来启动整个 Hadoop 服务**

所以，现在需要对文件权限进行授权

> 请确保已经提前创建好了 hadoop 用户（前置准备章节中有讲述），并配置好了 hadoop 用户之间的免密登录

- 以 **root 身份**，在 **node1、node2、node3 三台服务器** 上均执行如下命令

```bash
# 以root身份，在三台服务器上均执行
chown -R hadoop:hadoop /data
chown -R hadoop:hadoop /export
```

### 十、格式化整个文件系统

前期准备全部完成，现在对整个文件系统执行初始化

- 格式化 namenode

```bash
# 确保以hadoop用户执行
su - hadoop
# 格式化namenode
hadoop namenode -format
```

- 启动

```bash
# 一键启动hdfs集群
start-dfs.sh
# 一键关闭hdfs集群
stop-dfs.sh

# 如果遇到命令未找到的错误，表明环境变量未配置好，可以以绝对路径执行
/export/server/hadoop/sbin/start-dfs.sh
/export/server/hadoop/sbin/stop-dfs.sh
```

## 第五节 HDFS 的存储原理

### （一）进程启停管理

#### 一、一键启停脚本

Hadoop HDFS 组件内置了 HDFS 集群的一键启停脚本

- $HADOOP_HOME/sbin/start-dfs.sh，一键启动 HDFS 集群

  执行原理：

  - 在执行此脚本的机器上，启动 SecondaryNameNode
  - 读取 core-site.xml 内容（fs.defaultFS 项），确认 NameNode 所在机器，启动 NameNode

  - 读取 workers 内容，确认 DataNode 所在机器，启动全部 DataNode

- $HADOOP_HOME/sbin/stop-dfs.sh，一键关闭 HDFS 集群

  执行原理：

  - 在执行此脚本的机器上，关闭 SecondaryNameNode

  - 读取 core-site.xml 内容（fs.defaultFS 项），确认 NameNode 所在机器，关闭 NameNode

  - 读取 workers 内容，确认 DataNode 所在机器，关闭全部 NameNode

#### 二、单进程启停

除了一键启停外，也可以单独控制进程的启停

1. $HADOOP_HOME/sbin/hadoop-daemon.sh，此脚本可以单独控制**所在机器**的进程的启停

   用法：

   ```bash
   hadoop-daemon.sh (start|status|stop) (namenode|secondarynamenode|datanode)

2. $HADOOP_HOME/bin/hdfs，此程序也可以用以单独控制**所在机器**的进程的启停

   用法：

   ``` bash
   hdfs --daemon (start|status|stop) (namenode|secondarynamenode|datanode)
   ```

### （二）文件系统操作命令

#### 一、HDFS 文件系统基本信息

HDFS 作为分布式存储的文件系统，有其对数据的路径表达方式

- **HDFS 同 Linux 系统一样，均是以 / 作为根目录的组织形式**
- Linux：/usr/local/hello.txt
- HDFS：/usr/local/hello.txt

如何区分呢？

- Linux：file:///
- HDFS：hdfs://namenode:port/

如上路径：

- Linux：file:///usr/local/hello.txt
- HDFS：hdfs://node1:8020/usr/local/hello.txt

协议头 file:/// 或 hdfs://node1:8020 / **可以省略**

- 需要提供 Linux 路径的参数，会自动识别为 file:///
- 需要提供 HDFS 路径的参数，会自动识别为 hdfs:///

除非你明确需要写或不写会有 BUG，否则一般不用写协议头

#### 二、介绍

关于 HDFS 文件系统的操作命令，Hadoop 提供了 2 套命令体系

- hadoop 命令（老版本用法），用法：

  ``` bash
  hadoop fs [generic options]

- hdfs 命令（新版本用法），用法：

  ```bash
  hdfs dfs [generic options]
  ```

两者在文件系统操作上，用法完全一致，用哪个都可以

某些特殊操作需要选择 hadoop 命令或 hdfs 命令，讲到的时候具体分析

#### 三、创建文件夹

```bash
hadoop fs -mkdir [-p] <path> ...
```

```bash
hdfs dfs -mkdir [-p] <path> ...
```

- path 为待创建的目录
- -p 选项的行为与 Linux mkdir -p 一致，它会沿着路径创建父目录

#### 四、查看指定目录下内容

```bash
hadoop fs -ls [-h] [-R] [<path> ...]
```

```bash
hdfs dfs -ls [-h] [-R] [<path> ...]
```

- path 指定目录路径
- -h 人性化显示文件 size
- -R 递归查看指定目录及其子目录

#### 五、上传文件到 HDFS 指定目录下

```bash
hadoop fs -put [-f] [-p] <localsrc> ... <dst>
```

```bash
hdfs dfs -put [-f] [-p] <localsrc> ... <dst>
```

- -f 覆盖目标文件（已存在下）
- -p 保留访问和修改时间，所有权和权限。
- localsrc 本地文件系统（客户端所在机器）
- dst 目标文件系统（HDFS）

#### 六、查看 HDFS 文件内容

```bash
hadoop fs -cat <src> ...
```

```bash
hdfs dfs -cat <src> ...
```

读取指定文件全部内容，显示在标准输出控制台

读取大文件可以使用管道符配合 more

```bash
hadoop fs -cat <src> | more
```

```bash
hdfs dfs -cat <src> | more
```

#### 七、下载 HDFS 文件

```bash
hadoop fs -get [-f] [-p] <src> ... <localdst>
```

```bash
hdfs dfs -get [-f] [-p] <src> ... <localdst>
```

- 下载文件到本地文件系统指定目录，localdst 必须是目录
- -f 覆盖目标文件（已存在下）
- -p 保留访问和修改时间，所有权和权限

#### 八、拷贝 HDFS 文件

```bash
hadoop fs -cp [-f] <src> ... <dst>
```

```bash
hdfs dfs -cp [-f] <src> ... <dst>
```

- -f 覆盖目标文件（已存在的情况下）

#### 九、追加数据到 HDFS 文件中

```bash
hadoop fs -appendToFile <localsrc> ... <dst>
```

```bash
hdfs dfs -appendToFile <localsrc> ... <dst>
```

- 将所有给定本地文件的内容追加到给定 dst 文件
- dst 如果文件不存在，将创建该文件
- 如果 `<localSrc>` 为 -，则输入为从标准输入中读取

#### 十、HDFS 数据移动操作

```bash
hadoop fs -mv <src> ... <dst>
```

```bash
hdfs dfs -mv <src> ... <dst>
```

- 移动文件到指定文件夹下
- 可以使用该命令移动数据，重命名文件的名称

#### 十一、HDFS 数据删除操作

```bash
hadoop fs -rm -r [-skipTrash] URI [URI ...]
```

```bash
hdfs dfs -rm -r [-skipTrash] URI [URI ...]
```

- 删除指定路径的文件或文件夹
- -skipTrash 跳过回收站，直接删除

回收站功能默认关闭，如果要开启需要在 core-site.xml 内配置：

```xml
<property>
<name>fs.trash.interval</name>
<value>1440</value>
</property>

<property>
<name>fs.trash.checkpoint.interval</name>
<value>120</value>
</property>
```

无需重启集群，在哪个机器配置的，在哪个机器执行命令就生效

回收站默认位置在：/user/ 用户名(hadoop)/.Trash

#### 十二、HDFS WEB 浏览

除了使用命令操作 HDFS 文件系统外，在 HDFS 的 WEB UI 上也可以查看 HDFS 文件系统的内容

---

使用 WEB 浏览操作文件系统，一般会遇到权限问题

这是因为 WEB 浏览器中是以匿名用户（dr.who）登陆的，其只有只读权限，多数操作是做不了的

如果需要以特权用户在浏览器中进行操作，需要配置如下内容到 core-site.xml 并重启集群

```xml
<property>
  <name>hadoop.http.staticuser.user</name>
  <value>hadoop</value>
</property>
```

但是，**不推荐这样做**

- HDFS WEB UI，只读权限挺好的，简单浏览即可
- 如果给与高权限，会有很大的安全问题，造成数据泄露或丢失

### （三）HDFS Shell 命令权限不足问题解决

#### 一、Permission denied

想必有同学在实战 Shell 的时候，遇到了：

Permission denied: user = root, access = WRITE, inode = "/":hadoop:supergroup: drwxr-xr-x

问题的原因就是没有权限，那么为什么呢？

#### 二、HDFS 超级用户

HDFS 中，也是有权限控制的，其控制逻辑和 Linux 文件系统的完全一致

但是不同的是，大家的 Superuser 不同（超级用户不同）

- Linux 的超级用户是 root
- HDFS 文件系统的超级用户：是 **启动 namenode 的用户**（也就是课程的 hadoop 用户）

#### 三、修改权限

在 HDFS 中，可以使用和 Linux 一样的授权语句，即：chown 和 chmod

- 修改所属用户和组：

```bash
hadoop fs -chown [-R] root:root /xxx.txt
hdfs dfs -chown [-R] root:root /xxx.txt
```

- 修改权限

```bash
hadoop fs -chmod [-R] 777 /xxx.txt
hdfs dfs -chmod [-R] 777 /xxx.txt
```

### （四）HDFS 客户端-Jetbrains 产品插件

#### 一、Big Data Tools 插件

在 Jetbrains 的产品中，均可以安装插件，其中：**Big Data Tools** 插件可以帮助我们方便的操作 HDFS，比如

- IntelliJ IDEA（Java IDE）
- PyCharm（Python IDE）
- DataGrip（SQL IDE）

均可以支持 Bigdata Tool 插件

下面以 IDEA（PyCharm、DataGrip 均可）进行演示

#### 二、配置 Windows

需要对 Windows 系统做一些基础设置，配合插件使用

- 解压 Hadoop 安装包到 Windows 系统，如解压到：D:\hadoop-3.4.2
- 设置 $HADOOP_HOME 环境变量指向：D:\Tools\hadoop-3.4.2
- 下载

  - hadoop.dll（<https://github.com/steveloughran/winutils/blob/master/hadoop-3.0.0/bin/hadoop.dll>）
  - winutils.exe（<https://github.com/steveloughran/winutils/blob/master/hadoop-3.0.0/bin/winutils.exe>）
  
- 将 hadoop.dll 和 winutils.exe 放入 $HADOOP_HOME/bin 中

## 第六节 HDFS 的 Shell 操作

