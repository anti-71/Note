---
名称: 数据库连接池 Druid
章: 03 Java
节: "[[第五章 JDBC]]"
tags:
  - 知识点
index: 8
---
##### 数据库连接池

连接池是一个容器，负责分配和管理数据库连接，允许应用程序重复使用现有连接

**好处**：资源重用、提升响应速度、避免连接遗漏

标准接口：`DataSource`，功能：`Connection getConnection()`

##### Druid（德鲁伊）

阿里巴巴开源的数据库连接池，功能强大、性能优秀

##### 使用步骤

1. 导入 jar 包 `druid-1.1.12.jar`
2. 定义配置文件
3. 加载配置文件
4. 获取连接池对象
5. 获取连接

##### 代码

```java
Properties prop = new Properties();
prop.load(new FileInputStream("src/druid.properties"));
DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
Connection conn = dataSource.getConnection();
```
