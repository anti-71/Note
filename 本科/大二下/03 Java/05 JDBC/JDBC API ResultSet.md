---
名称: JDBC API ResultSet
章: 03 Java
节: "[[第五章 JDBC]]"
tags:
  - 知识点
index: 6
---
##### 作用

封装 DQL 查询语句的结果

##### 方法

| 方法 | 说明 |
|:--|:--|
| `boolean next()` | 光标下移一行，判断是否为有效行 |
| `xxx getXxx(参数)` | 获取数据，参数为列编号（从 1 开始）或列名 |

##### 遍历

```java
ResultSet rs = stmt.executeQuery("select * from account");
while (rs.next()) {
    int id = rs.getInt(1);
    String name = rs.getString(2);
    double money = rs.getDouble(3);
    System.out.println(id + " " + name + " " + money);
}
```

##### 封装对象

```java
List<Account> list = new ArrayList<>();
while (rs.next()) {
    Account account = new Account();
    account.setId(rs.getInt("id"));
    account.setName(rs.getString("name"));
    account.setMoney(rs.getDouble("money"));
    list.add(account);
}
```
