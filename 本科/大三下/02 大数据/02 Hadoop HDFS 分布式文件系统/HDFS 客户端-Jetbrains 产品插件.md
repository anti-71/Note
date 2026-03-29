---
名称: HDFS 客户端-Jetbrains 产品插件
章: 02 大数据
节: "[[第二章 Hadoop HDFS 分布式文件系统]]"
tags:
  - 知识点
index: 8
---
##### Big Data Tools 插件

在 Jetbrains 的产品中，均可以安装插件，其中：**Big Data Tools** 插件可以帮助我们方便的操作 HDFS，比如

- IntelliJ IDEA（Java IDE）
- PyCharm（Python IDE）
- DataGrip（SQL IDE）

均可以支持 Bigdata Tool 插件

下面以 IDEA（PyCharm、DataGrip 均可）进行演示

##### 配置 Windows

需要对 Windows 系统做一些基础设置，配合插件使用

- 解压 Hadoop 安装包到 Windows 系统，如解压到：D:\hadoop-3.4.2
- 设置 $HADOOP_HOME 环境变量指向：D:\Tools\hadoop-3.4.2
- 下载

  - hadoop.dll（<https://github.com/steveloughran/winutils/blob/master/hadoop-3.0.0/bin/hadoop.dll>）
  - winutils.exe（<https://github.com/steveloughran/winutils/blob/master/hadoop-3.0.0/bin/winutils.exe>）
  
- 将 hadoop.dll 和 winutils.exe 放入 $HADOOP_HOME/bin 中