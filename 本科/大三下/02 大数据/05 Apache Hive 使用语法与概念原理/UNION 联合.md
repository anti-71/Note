---
名称: UNION 联合
章: 02 大数据
节: "[[第五章 Apache Hive 使用语法与概念原理]]"
tags:
  - 知识点
index: 13
---
#####  UNION 联合

**UNION** 用于将多个 SELECT 语句的结果组合成单个结果集

每个 select 语句返回的列的数量和名称必须相同，否则，将引发架构错误

- **基础语法**：

```sql
SELECT ...
UNION [ALL]
SELECT ...
```

###### 准备数据进行测试

```sql
CREATE TABLE itheima.course(
    c_id string,
    c_name string,
    t_id string
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t';

LOAD DATA LOCAL INPATH '/home/hadoop/course.txt' INTO TABLE itheima.course;
```

- 联合两个查询结果集

```sql
SELECT * FROM course WHERE t_id = '周杰伦'
UNION
SELECT * FROM course WHERE t_id = '王力宏'
```

###### 其它写法

- UNION 写在 FROM 中

```sql
SELECT t_id, COUNT(*) FROM
(
    SELECT t_id FROM itheima.course WHERE t_id = '周杰伦'
    UNION ALL
    SELECT t_id FROM itheima.course WHERE t_id = '王力宏'
) AS u GROUP BY t_id;
```