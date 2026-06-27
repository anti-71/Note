---
名称: JDBC 快速入门
章: 03 Java
节: "[[第五章 JDBC]]"
tags:
  - 知识点
index: 2
---
##### 步骤

1. 注册驱动：`Class.forName("com.mysql.cj.jdbc.Driver")`
2. 获取连接：`DriverManager.getConnection(url, username, password)`
3. 定义 SQL 语句
4. 获取执行 SQL 对象：`Statement`
5. 执行 SQL
6. 处理返回结果
7. 释放资源

##### 代码

```java
Class.forName("com.mysql.cj.jdbc.Driver");

String url = "jdbc:mysql://127.0.0.1:3306/db1";
String username = "root";
String password = "1234";
Connection conn = DriverManager.getConnection(url, username, password);

String sql = "update account set money = 2000 where id = 1";
Statement stmt = conn.createStatement();
int count = stmt.executeUpdate(sql);
System.out.println(count);

stmt.close();
conn.close();
```
