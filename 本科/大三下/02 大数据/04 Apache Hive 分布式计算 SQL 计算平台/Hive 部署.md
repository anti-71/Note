---
名称: Hive 部署
章: 02 大数据
节: "[[第四章 Apache Hive 分布式计算 SQL 计算平台]]"
tags:
  - 知识点
index: 4
---
> [!question] Hive 是分布式运行的框架还是单机运行的？
> Hive 是单机工具，只需要部署在一台服务器即可
> 
> Hive 虽然是单机的，但是它可以提交分布式运行的 MapReduce 程序运行

##### 规划

我们知道 Hive 是单机工具后，就需要准备一台服务器供 Hive 使用即可

同时 Hive 需要使用元数据服务，即需要提供一个关系型数据库，我们也选择一台服务器安装关系型数据库即可

所以：

|服务|机器|
|---|---|
|Hive 本体|部署在 node 1|
|元数据服务所需的关系型数据库（课程选择最为流行的 **MySQL**）|部署在 node 1|

为了简单起见，都安装到 node 1 服务器上

##### 步骤 1：安装 MySQL 数据库

我们在 node 1 节点使用 yum 在线安装 MySQL 5.7 版本

由于操作系统内置了 MySQL 8.0 的版本，我们可以直接从启动 mysql 开始

```bash
# 更新密钥
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
# 安装Mysql yum库
rpm -Uvh http://repo.mysql.com//mysql57-community-release-el7-7.noarch.rpm
# yum安装Mysql
yum -y install mysql-community-server
# 启动Mysql设置开机启动
systemctl start mysqld
systemctl enable mysqld
# 检查Mysql服务状态
systemctl status mysqld
# 第一次启动mysql，会在日志文件中生成root用户的一个随机密码，使用下面命令查看该密码
grep 'temporary password' /var/log/mysqld.log
# 如果你想设置简单密码，需要降低Mysql的密码安全级别
set global validate_password_policy=LOW; # 密码安全级别低
set global validate_password_length=4;   # 密码长度最低4位即可
# 然后就可以用简单密码了（课程中使用简单密码，为了方便，生产中不要这样）
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
grant all privileges on *.* to root@"%" identified by 'root' with grant option;
flush privileges;
```

##### 步骤 2：配置 Hadoop

Hive 的运行依赖于 Hadoop（HDFS、MapReduce、YARN 都依赖）

同时涉及到 HDFS 文件系统的访问，所以需要配置 Hadoop 的代理用户

即设置 hadoop 用户允许代理（模拟）其它用户

配置如下内容在 Hadoop 的 core-site.xml 中，并分发到其它节点，且重启 HDFS 集群

```xml
<property>
  <name>hadoop.proxyuser.hadoop.hosts</name>
  <value>*</value>
</property>
<property>
  <name>hadoop.proxyuser.hadoop.groups</name>
  <value>*</value>
</property>
```

##### 步骤 3：下载解压 Hive

- 切换到 hadoop 用户

```bash
su - hadoop
```

- 下载 Hive 安装包：

```
http://archive.apache.org/dist/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
```

- 解压到 node 1 服务器的：/export/server/ 内

```bash
tar -zxvf apache-hive-3.1.3-bin.tar.gz -C /export/server/
```

- 设置软连接

```bash
ln -s /export/server/apache-hive-3.1.3-bin /export/server/hive
```

##### 步骤 4：提供 MySQL Driver 包

- 下载 MySQL 驱动包：

```
https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.34/mysql-connector-java-5.1.34.jar
```

- 将下载好的驱动 jar 包，放入：Hive 安装文件夹的 lib 目录内

```bash
mv mysql-connector-java-5.1.34.jar /export/server/hive/lib/
```

##### 步骤 5：配置 Hive

- 在 Hive 的 conf 目录内，新建 hive-env.sh 文件，填入以下环境变量内容：
 
```bash
export HADOOP_HOME=/export/server/hadoop
export HIVE_CONF_DIR=/export/server/hive/conf
export HIVE_AUX_JARS_PATH=/export/server/hive/lib
```

- 在 Hive 的 conf 目录内，新建 hive-site.xml 文件，填入以下内容：

```xml
<configuration>
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:mysql://node1:3306/hive?createDatabaseIfNotExist=true&amp;useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8</value>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.jdbc.Driver</value>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>root</value>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>123456</value>
  </property>
  <property>
    <name>hive.server2.thrift.bind.host</name>
    <value>node1</value>
  </property>
  <property>
    <name>hive.metastore.uris</name>
    <value>thrift://node1:9083</value>
  </property>
  <property>
    <name>hive.metastore.event.db.notification.api.auth</name>
    <value>false</value>
  </property>
</configuration>
```

##### 步骤 6：初始化元数据库

支持，Hive 的配置已经完成，现在在启动 Hive 前，需要先初始化 Hive 所需的元数据库。

- 在 MySQL 中新建数据库：hive

```sql
CREATE DATABASE hive CHARSET UTF8;
```

- 执行元数据库初始化命令：

```bash
cd /export/server/hive
bin/schematool -initSchema -dbType mysql -verbose
```

初始化成功后，会在 MySQL 的 hive 库中新建 74 张元数据管理的表

##### 步骤 7：启动 Hive（使用 Hadoop 用户）

- 确保 Hive 文件夹所属为 hadoop 用户
- 创建一个 hive 的日志文件夹：

```bash
mkdir /export/server/hive/logs
```

- 启动元数据管理服务（必须启动，否则无法工作）

前台启动：

```bash
bin/hive --service metastore
```

后台启动：

```bash
nohup bin/hive --service metastore >> logs/metastore.log 2>&1 &
```

- 启动客户端，二选一（当前先选择 Hive Shell 方式）

Hive Shell 方式（可以直接写 SQL）：

```bash
bin/hive
```

Hive ThriftServer 方式（不可直接写 SQL，需要外部客户端链接使用）：

```bash
bin/hive --service hiveserver2
```