---
名称: JDBC API DriverManager
章: 03 Java
节: "[[第五章 JDBC]]"
tags:
  - 知识点
index: 3
---
##### DriverManager 作用

1. 注册驱动（可省略）
2. 获取数据库连接

##### 获取连接

```java
static Connection getConnection(String url, String user, String password)
```

##### URL 格式

```
jdbc:mysql://ip地址:端口号/数据库名?参数键值对
```

- 本机 + 默认端口 3306 可简写：`jdbc:mysql:///db1`
- 配置 `useSSL=false` 禁用安全连接，解决警告

##### 代码

```java
String url = "jdbc:mysql:///db1";
Connection conn = DriverManager.getConnection(url, "root", "1234");
```
