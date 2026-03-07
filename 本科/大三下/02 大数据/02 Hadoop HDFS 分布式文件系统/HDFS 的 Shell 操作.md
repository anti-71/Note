---
名称: HDFS 的 Shell 操作
章: 02 大数据
节: "[[第二章 Hadoop HDFS 分布式文件系统]]"
tags:
  - 知识点
index: 5
---
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