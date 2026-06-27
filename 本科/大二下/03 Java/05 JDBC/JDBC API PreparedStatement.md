---
名称: JDBC API PreparedStatement
章: 03 Java
节: "[[第五章 JDBC]]"
tags:
  - 知识点
index: 7
---
##### SQL 注入

通过操作输入修改事先定义好的 SQL 语句，对服务器进行攻击

```java
// 攻击示例：密码输入 ' or '1' = '1
String pwd = "' or '1' = '1";
String sql = "select * from tb_user where username = '" + name + "' and password = '" + pwd + "'";
// 此 SQL 恒成立，绕过登录验证
```

##### PreparedStatement 作用

预编译 SQL 语句并执行，预防 SQL 注入

##### 步骤

1. SQL 中使用 `?` 占位符
2. 通过 `conn.prepareStatement(sql)` 获取对象
3. 使用 `setXxx(参数1, 参数2)` 给 `?` 赋值
4. 执行 `executeUpdate()` / `executeQuery()`，不需再传 SQL

##### 代码

```java
String sql = "select * from tb_user where username = ? and password = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, name);
pstmt.setString(2, pwd);
ResultSet rs = pstmt.executeQuery();
```

##### 好处

- 预编译 SQL，性能更高（开启：`useServerPrepStmts=true`）
- 防止 SQL 注入，将敏感字符转义
