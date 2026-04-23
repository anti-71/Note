---
名称: Sampling 采样
章: 02 大数据
节: "[[第五章 Apache Hive 使用语法与概念原理]]"
tags:
  - 知识点
index: 14
---
##### 为什么需要抽样表数据

对表进行随机抽样是非常有必要的

大数据体系下，在真正的企业环境中，很容易出现很大的表，比如体积达到 TB 级别

对这种表一个简单的 SELECT * 都会非常的慢，哪怕 LIMIT 10 想要看 10 条数据，也会走 MapReduce 流程，这个时间等待是不合适的

Hive 提供的快速抽样的语法，可以快速从大表中随机抽取一些数据供用户查看

##### TABLESAMPLE 函数

进行随机抽样，本质上就是用 TABLESAMPLE 函数

###### 语法 1：基于随机分桶抽样

```sql
SELECT ... FROM tbl TABLESAMPLE(BUCKET x OUT OF y ON(colname | rand()))
```

- y 表示将表数据随机划分成 y 份（y 个桶）
- x 表示从 y 里面随机抽取 x 份数据作为取样
- colname 表示随机的依据基于某个列的值
- rand () 表示随机的依据基于整行

> [!example] 示例
> ```sql
> SELECT username, orderId, totalmoney FROM itheima.orders TABLESAMPLE(BUCKET 1 OUT OF 10 ON username);
> SELECT * FROM itheima.orders TABLESAMPLE(BUCKET 1 OUT OF 10 ON rand());
> ```

> [!warning] 注意
> - 使用 colname 作为随机依据，则其它条件不变下，每次抽样结果一致
> - 使用 rand() 作为随机依据，每次抽样结果都不同

###### 语法 2：基于数据块抽样

```sql
SELECT ... FROM tbl TABLESAMPLE(num ROWS | num PERCENT | num(K|M|G));
```

- num ROWS 表示抽样 num 条数据
- num PERCENT 表示抽样 num 百分比例的数据
- num (K|M|G) 表示抽取 num 大小的数据，单位可以是 K、M、G 表示 KB、MB、GB

> [!warning] 注意
> 使用这种语法抽样，条件不变的话，每一次抽样的结果都一致，即：
> 
> 无法做到随机，只是按照数据顺序从前向后取