---
名称: fsck 命令
章: 02 大数据
节: "[[第二章 Hadoop HDFS 分布式文件系统]]"
tags:
  - 知识点
index: 10
---
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
