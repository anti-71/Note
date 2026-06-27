---
名称: JDBC API Connection
章: 03 Java
节: "[[第五章 JDBC]]"
tags:
  - 知识点
index: 4
---
##### 获取执行 SQL 的对象

| 方法 | 说明 |
|:--|:--|
| `createStatement()` | 普通执行 SQL 对象 |
| `prepareStatement(sql)` | 预编译 SQL，防止 SQL 注入 |
| `prepareCall(sql)` | 执行存储过程 |

##### 事务管理

MySQL 默认自动提交事务，JDBC 中通过 Connection 管理：

| 方法 | 说明 |
|:--|:--|
| `setAutoCommit(boolean)` | `false` 为手动提交（开启事务） |
| `commit()` | 提交事务 |
| `rollback()` | 回滚事务 |

##### 代码

```java
conn.setAutoCommit(false); // 开启事务
try {
    stmt.executeUpdate(sql1);
    stmt.executeUpdate(sql2);
    conn.commit(); // 提交
} catch (Exception e) {
    conn.rollback(); // 回滚
    e.printStackTrace();
}
```
