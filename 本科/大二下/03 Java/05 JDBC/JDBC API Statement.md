---
名称: JDBC API Statement
章: 03 Java
节: "[[第五章 JDBC]]"
tags:
  - 知识点
index: 5
---
##### 作用

执行 SQL 语句

| 方法 | 说明 |
|:--|:--|
| `int executeUpdate(sql)` | 执行 DML、DDL，返回受影响行数 |
| `ResultSet executeQuery(sql)` | 执行 DQL，返回结果集对象 |

##### 代码

```java
// DML
String sql = "update account set money = 3000 where id = 1";
int count = stmt.executeUpdate(sql);
if (count > 0) System.out.println("执行成功");

// DDL
String sql = "drop database db2";
int count = stmt.executeUpdate(sql);
```
