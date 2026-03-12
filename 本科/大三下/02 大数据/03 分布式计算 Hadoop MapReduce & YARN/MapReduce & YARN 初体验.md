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

##### 提交 MapReduce 程序至 YARN 运行

在部署并成功启动 YARN 集群后，我们就可以在 YARN 上运行各类应用程序了

YARN 作为资源调度管控框架，其本身提供资源供许多程序运行，常见的有：

- MapReduce 程序
- Spark 程序
- Flink 程序

Spark 和 Flink 是大数据后续的学习内容，我们目前先来体验一下在 YARN 上执行 MapReduce 程序的过程

Hadoop 官方内置了一些预置的 MapReduce 程序代码，我们无需编程，只需要通过命令即可使用

常用的有 2 个 MapReduce 内置程序：

- wordcount：单词计数程序
	- 统计指定文件内各个单词出现的次数
- pi：求圆周率
    - 通过蒙特卡罗算法（统计模拟法）求圆周率

这些内置的示例 MapReduce 程序代码，都在：

- `$HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar` 这个文件内

可以通过 `hadoop jar` 命令来运行它，提交 MapReduce 程序到 YARN 中

语法：
```bash
hadoop jar 程序文件 java类名 [程序参数] ... [程序参数]
```

##### 提交 word-count 示例程序

单词计数示例程序的功能很简单：

- 给定数据输入的路径（HDFS）、给定结果输出的路径（HDFS）
- 将输入路径内的数据中的单词进行计数，将结果写到输出路径

我们可以准备一份数据文件，并上传到 HDFS 中

```
itheima itcast itheima itcast
hadoop hdfs hadoop hdfs
hadoop mapreduce hadoop yarn
itheima hadoop itcast hadoop
itheima itcast hadoop yarn mapreduce
```

将左侧内容保存到 Linux 中为 words.txt 文件，并上传到 HDFS：

```bash
hadoop fs -mkdir -p /input/wordcount
hadoop fs -mkdir /output
hadoop fs -put words.txt /input/wordcount/
```

执行如下命令，提交示例 MapReduce 程序 WordCount 到 YARN 中执行：

```bash
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar wordcount hdfs://node1:8020/input/wordcount/ hdfs://node1:8020/output/wc1
```

> [!warning] 注意
> - 参数 wordcount，表示运行 jar 包中的单词计数程序（Java Class）
> - 参数 1 是数据输入路径（hdfs://node1:8020/input/wordcount/）
> - 参数 2 是结果输出路径（hdfs://node1:8020/output/wc1），**需要确保输出的文件夹不存在**
