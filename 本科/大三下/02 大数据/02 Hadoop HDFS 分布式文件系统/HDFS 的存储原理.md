---
名称: HDFS 的存储原理
章: 02 大数据
节: "[[第二章 Hadoop HDFS 分布式文件系统]]"
tags:
  - 知识点
index: 6
---
### （一）存储原理

##### HDFS 分布式文件存储

**分布式存储**：每个服务器（节点）存储文件的一部分

> [!question] 问题
> 文件大小不一，不利于统一管理

**解决**：设定统一的管理单位，block 块

- Block 块，HDFS 最小存储单位，每个 256MB（可以修改）

> [!question] 问题
> 如果丢失或损坏了某个 Block 块呢？
>
> >丢失一个 Block 块就导致文件不完整了
> >
> >Block 块越多，损坏的几率越大

**解决**：通过多个副本（备份）

- 每个 Block 块都有 2 个（可修改）备份
- 每个副本都复制到其它服务器一份
- 每个块都有 2 个备份在其它服务器上

### （二）fsck 命令

##### HDFS 副本块数量的配置

在前面我们了解了 HDFS 文件系统的数据安全，是依靠多个副本来确保的

如何设置默认文件上传到 HDFS 中拥有的副本数量呢？

可以在 `hdfs-site.xml` 中配置如下属性：

```xml
<property>
    <name>dfs.replication</name>
    <value>3</value>
</property>
```

这个属性默认是 3，一般情况下，我们无需主动配置（除非需要设置非 3 的数值）

如果需要自定义这个属性，请修改每一台服务器的 `hdfs-site.xml` 文件，并设置此属性

------

- 除了配置文件外，我们还可以在上传文件的时候，临时决定被上传文件以多少个副本存储

  ```bash
  hadoop fs -D dfs.replication=2 -put test.txt /tmp/
  ```

  如上命令，就可以在上传 `test.txt` 的时候，临时设置其副本数为 2

- 对于已经存在 HDFS 的文件，修改 `dfs.replication` 属性不会生效，如果要修改已存在文件可以通过命令：

  ```bash
  hadoop fs -setrep [-R] 2 path
  ```

  如上命令，指定 `path` 的内容将会被修改为 2 个副本存储

  `-R` 选项可选，使用 `-R` 表示对子目录也生效

##### fsck 命令检查文件的副本数

同时，我们可以使用 HDFS 提供的 `fsck` 命令来检查文件的副本数：

```bash
hdfs fsck path [-files [-blocks [-locations]]]
```

`fsck` 可以检查指定路径是否正常：

- `-files` 可以列出路径内的文件状态
- `-files -blocks` 输出文件块报告（有几个块，多少副本）
- `-files -blocks -locations` 输出每一个 block 的详情

##### block 配置

可以看到通过 `fsck` 命令我们验证了：

- 文件有多个副本
- 文件被分成多个块存储在 HDFS

对于块（block），HDFS 默认设置为 256MB 一个，也就是 1GB 文件会被划分为 4 个 block 存储

块大小可以通过参数：

```xml
<property>
    <name>dfs.blocksize</name>
    <value>268435456</value>
    <description>设置HDFS块大小，单位是b</description>
</property>
```

如上，设置为 256MB

### （三）NameNode 元数据

##### edits 文件

在 HDFS 中，文件是被划分了一堆堆的 block 块，那如果文件很大、以及文件很多，Hadoop 是如何记录和整理文件和 block 块的关系呢？

答案就在于 NameNode

**edits 文件**，是一个流水账文件，记录了 HDFS 中的每一次操作，以及本次操作影响的文件其对应的 block

所以，会存在多个 edits 文件：

- 确保不会有超大 edits 的存在
- 保证检索性能

问题在于，当用户想要查看某文件内容，如：`/tmp/data/test.txt`，就需要在全部的 edits 中搜索（还需要按顺序从头到尾，避免后期改名或删除），效率非常低

需要 **合并** edits 文件，得到最终的结果

##### fsimage 文件

将全部的 edits 文件，合并为最终结果，即可得到一个 FSImage 文件

##### NameNode 元数据管理维护

NameNode 基于 edits 和 FSImage 的配合，完成整个文件系统文件的管理

1. 每次对 HDFS 的操作，均被 edits 文件记录

2. edits 达到大小上线后，开启新的 edits 记录

3. 定期进行 edits 的合并操作

   - 如当前没有 fsimage 文件，将全部 edits 合并为第一个 fsimage
   - 如当前已存在 fsimage 文件，将全部 edits 和已存在的 fsimage 进行合并，形成新的 fsimage



4. 重复 123 流程

##### 元数据合并控制参数

对于元数据的合并，是一个定时过程，基于：

- `dfs.namenode.checkpoint.period`，默认 3600（秒）即 1 小时
- `dfs.namenode.checkpoint.txns`，默认 1000000，即 100W 次事务

只要有一个达到条件就执行

检查是否达到条件，默认 60 秒检查一次，基于：

- `dfs.namenode.checkpoint.check.period`，默认 60（秒），来决定

##### SecondaryNameNode 的作用

对于元数据的合并，还记得 HDFS 集群有一个辅助角色：SecondaryNameNode 吗？

没错，合并元数据的事情就是它干的

SecondaryNameNode 会通过 http 从 NameNode 拉取数据（edits 和 fsimage），然后合并完成后提供给 NameNode 使用

### （四）HDFS 数据的读写流程

##### 数据写入流程

1. 客户端向 NameNode 发起请求
2. NameNode 审核权限、剩余空间后，满足条件允许写入，并告知客户端写入的 DataNode 地址
3. 客户端向指定的 DataNode 发送数据包
4. 被写入数据的 DataNode 同时完成数据副本的复制工作，将其接收的数据分发给其它 DataNode
5. 例如，DataNode1 复制给 DataNode2，然后基于 DataNode2 复制给 Datanode3 和 DataNode4
6. 写入完成客户端通知 NameNode，NameNode 做元数据记录工作

**关键信息点**：

- NameNode 不负责数据写入，只负责元数据记录和权限审批
- 客户端直接向 1 台 DataNode 写数据，这个 DataNode 一般是离客户端最近（网络距离）的那一个
- 数据块副本的复制工作，由 DataNode 之间自行完成（构建一个 PipLine，按顺序复制分发）

##### 数据读取流程

1. 客户端向 NameNode 申请读取某文件
2. NameNode 判断客户端权限等细节后，允许读取，并返回此文件的 block 列表
3. 客户端拿到 block 列表后自行寻找 DataNode 读取即可

**关键点**：

1. 数据同样不通过 NameNode 提供

2. NameNode 提供的 block 列表，会基于网络距离计算尽量提供离客户端最近的

   这是因为 1 个 block 有 3 份，会尽量找离客户端最近的那一份让其读取
