---
名称: HDFS Shell 命令权限不足问题解决
章: 02 大数据
节: "[[第二章 Hadoop HDFS 分布式文件系统]]"
tags:
  - 知识点
index: 7
---
##### Permission denied

想必有同学在实战 Shell 的时候，遇到了：

Permission denied: user = root, access = WRITE, inode = "/":hadoop:supergroup: drwxr-xr-x

问题的原因就是没有权限，那么为什么呢？

##### HDFS 超级用户

HDFS 中，也是有权限控制的，其控制逻辑和 Linux 文件系统的完全一致

但是不同的是，大家的 Superuser 不同（超级用户不同）

- Linux 的超级用户是 root
- HDFS 文件系统的超级用户：是 **启动 namenode 的用户**（也就是课程的 hadoop 用户）

##### 修改权限

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
