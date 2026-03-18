---
名称: Hive 初体验
章: 02 大数据
节: "[[第四章 Apache Hive 分布式计算 SQL 计算平台]]"
tags:
  - 知识点
index: 5
---
##### Hive 体验

首先，确保启动了 Metastore 服务

可以执行：`bin/hive`，进入到 Hive Shell 环境中，可以直接执行 SQL 语句

- 创建表

```sql
CREATE TABLE test(id INT, name STRING, gender STRING);
```

- 插入数据

```sql
INSERT INTO test VALUES(1, '王力红', '男'), (2, '周杰伦', '男'), (3, '林志灵', '女');
```

- 查询数据

```sql
SELECT gender, COUNT(*) AS cnt FROM test GROUP BY gender;
```