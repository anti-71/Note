---
名称: NameNode 元数据
章: 02 大数据
节: "[[第二章 Hadoop HDFS 分布式文件系统]]"
tags:
  - 知识点
index: 11
---
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
