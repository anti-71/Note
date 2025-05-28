# JDBC

## 第一节 JDBC简介

**JDBC概念：**

JDBC就是使用Java语言操作关系型数据库的一套API

全称：（Java DataBase Connectivity）Java数据库连接

**JDBC本质：**

官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口

各个数据库厂商去实现这套接口，提供数据库驱动jar包

我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类

**JDBC好处：**

各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发

可随时替换底层数据库，访问数据库的Java代码基本不变

## 第二节 JDBC快速入门

**步骤**

0.创建工程，导入驱动jar包

1.注册驱动

`Class.forName("com.mysql.cj.jdbc.Driver");`

2.获取连接

`Connection conn DriverManager.getConnection(url,username,password);`

3.定义SQL语句

`String sql "update...";`

4.获取执行SQL对象

`Statement stmt conn.createStatement（）;`

5.执行SQL

`stmt.executeUpdate(sql);`

6.处理返回结果

7.释放资源

**代码**

```java
// 目的：JDBC快速入门
        // 1、注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver");

        // 2、获取连接
        String url = "jdbc:mysql://127.0.0.1:3306/db1";
        String username = "root";
        String password = "1234";
        Connection conn = DriverManager.getConnection(url, username, password);

        // 3、定义sql
        String sql = "update account set money = 2000 where id = 1";

        // 4、获取执行sql对象 Statement
        Statement stmt = conn.createStatement();

        // 5、执行sql
        int count = stmt.executeUpdate(sql); // 受影响的行数

        // 6、处理结果
        System.out.println(count);

        // 7、释放资源
        stmt.close();
        conn.close();
```

## 第三节 JDBC API详解

### 1.DriverManager
**DriverManager（驱动管理类）作用：**

1. 注册驱动
   可以省略注册驱动的步骤

2. 获取数据库连接
   `static Connection``getConnection(String url, String user, String password)`

   **参数**
   1.url：连接路径
   	语法：jdbc:mysq://ip地址（域名）:端口号/数据库名称？参数键值对1&参数键值对2.
   	示例：jdbc:mysql:/127.0.0.1:3306/db1
   	细节：
   		如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则u可以简写为：jdbc:mysql:///数据库名称？参数键值对
   		配置useSSL=false参数，禁用安全连接方式，解决警告提示
   2.user：用户名
   3.password：密码

**代码**

```java
// 2、获取连接：如果连接的是本机mysql并且端口是默认的3306可以简化书写
        String url = "jdbc:mysql:///db1";
```

### 2.Connection

**Connection（数据库连接对象）作用：**

1. 获取执行SQL的对象

   普通执行SQL对象

   `Statement createStatement()`

   预编译SQL的执行SQL对象：防止SQL注入

   `PreparedStatement prepareStatement(sql)`

   执行存储过程的对象

   `CallableStatement prepareCall(sql)`

2. 管理事务

   MySQL事务管理

   开启事务：BEGIN;/START TRANSACTION;

   提交事务：COMMIT;

   回滚事务：ROLLBACK;

   MySQL默认自动提交事务

   JDBC事务管理：Connection接口中定义了3个对应的方法

   开启事务：setAutoCommit(boolean autoCommit)：true为自动提交事务；false为手动提交事务，即为开启事务

   提交事务：commit()

   回滚事务：rollback()

   **代码**

   ```java
   // 目的：JDBC API 详解：Connection
           // 2、获取连接：如果连接的是本机mysql并且端口是默认的3306可以简化书写
           String url = "jdbc:mysql:///db1";
           String username = "root";
           String password = "1234";
           Connection conn = DriverManager.getConnection(url, username, password);
   
           // 3、定义sql
           String sql1 = "update account set money = 3000 where id = 1";
           String sql2 = "update account set money = 3000 where id = 2";
   
           // 4、获取执行sql对象 Statement
           Statement stmt = conn.createStatement();
   
           try {
               // 开启事务
               conn.setAutoCommit(false);
               // 5、执行sql
               int count1 = stmt.executeUpdate(sql1); // 受影响的行数
               // 6、处理结果
               System.out.println(count1);
   
               int i = 1 / 0;
   
               // 5、执行sql
               int count2 = stmt.executeUpdate(sql2); // 受影响的行数
               // 6、处理结果
               System.out.println(count2);
   
               // 提交事务
               conn.commit();
           } catch (Exception e) {
               // 回滚事务
               conn.rollback();
               e.printStackTrace();
           }
   
           // 7、释放资源
           stmt.close();
           conn.close();
   ```


### 3.Statement

**Statement作用：**

1. 执行SQL语句

   `int executeUpdate(sql)`：执行DML、DDL语句

   返回值：（1）DML语句影响的行数（2）DDL语句执行后，执行成功也可能返回0

   ResultSet executeQuery(sql)：执行DQL语句

   返回值：ResultSet结果集对象

**代码**

```java
// 目的：JDBC API 详解：Statement

    // 执行DML语句
    @Test
    public void testDML() throws Exception{
        // 2、获取连接：如果连接的是本机mysql并且端口是默认的3306可以简化书写
        String url = "jdbc:mysql:///db1";
        String username = "root";
        String password = "1234";
        Connection conn = DriverManager.getConnection(url, username, password);

        // 3、定义sql
        String sql1 = "update account set money = 3000 where id = 5";

        // 4、获取执行sql对象 Statement
        Statement stmt = conn.createStatement();

        // 5、执行sql
        int count1 = stmt.executeUpdate(sql1); // 执行完DDL语句，可能是0

        // 6、处理结果
        if (count1 > 0){
            System.out.println("执行成功！");
        } else {
            System.out.println("执行失败！");
        }

        // 7、释放资源
        stmt.close();
        conn.close();
    }

    // 执行DDL语句
    @Test
    public void testDDL() throws Exception{
        // 2、获取连接：如果连接的是本机mysql并且端口是默认的3306可以简化书写
        String url = "jdbc:mysql:///db1";
        String username = "root";
        String password = "1234";
        Connection conn = DriverManager.getConnection(url, username, password);

        // 3、定义sql
        String sql = "drop database db2";

        // 4、获取执行sql对象 Statement
        Statement stmt = conn.createStatement();

        // 5、执行sql
        int count = stmt.executeUpdate(sql); // 执行完DML语句后，受影响的行数

        // 6、处理结果
        System.out.println(count);

        // 7、释放资源
        stmt.close();
        conn.close();
    }
```

### 4.ResultSet

**ResultSet（结果集对象）作用：**

1. 封装了DQL查询语句的结果

   	`ResultSet stmt.executeQuery(sql)`:执行DQL语句，返回ResultSet对象
   获取查询结果

   `boolean next()`：（1）将光标从当前位置向前移动一行（2）判断当前行是否为有效行

   返回值：

   true:有效行，当前行有数据

   false:无效行，当前行没有数据

   `xxx getXxx(参数)`：获取数据

   xxx：数据类型；如：int getlnt（参数）；String getString（参数）

   参数：

   int：列的编号，从1开始

   String：列的名称

**使用步骤：**

1. 游标向下移动一行，并判断该行否有数据：next()

2. 获取数据：getXxx(参数)

   ```java
   // 循环判断游标是否是最后一行末尾
   while(rs.next() {
   	// 获取数据
   	rs.getXxx(参数);
   }
   ```

**代码**

```java
// 目的：JDBC API 详解：ResultSet

    // 执行DQL语句
    @Test
    public void testResultSet() throws Exception {
        // 2、获取连接：如果连接的是本机mysql并且端口是默认的3306可以简化书写
        String url = "jdbc:mysql:///db1";
        String username = "root";
        String password = "1234";
        Connection conn = DriverManager.getConnection(url, username, password);

        // 3、定义sql
        String sql = "select * from account";

        // 4、获取statement对象
        Statement stmt = conn.createStatement();

        // 5、执行sql
        ResultSet rs = stmt.executeQuery(sql);

        // 6、处理结果，遍历rs中的所有数据
        // 光标向下移动一行，并且判断当前行是否有数据
        while (rs.next()) {
            // 获取数据 getXxx()
            int id = rs.getInt(1);
            String name = rs.getString(2);
            double money = rs.getDouble(3);

            System.out.println(id);
            System.out.println(name);
            System.out.println(money);

            System.out.println("---------------------------------------------");
        }

        // 7、释放资源
        rs.close();
        stmt.close();
        conn.close();
    }
```

**实例**

```java
/**
     * 查询account账户表数据，封装为Account对象中，并且存储到ArrayList集合中
     * 1、定义实体类Account
     * 2、查询数据，封装到Account对象中
     * 3、将Account对象添加到ArrayList集合中
     */
    @Test
    public void testResultSet2() throws Exception {
        // 2、获取连接：如果连接的是本机mysql并且端口是默认的3306可以简化书写
        String url = "jdbc:mysql:///db1";
        String username = "root";
        String password = "1234";
        Connection conn = DriverManager.getConnection(url, username, password);

        // 3、定义sql
        String sql = "select * from account";

        // 4、获取statement对象
        Statement stmt = conn.createStatement();

        // 5、执行sql
        ResultSet rs = stmt.executeQuery(sql);

        // 创建集合
        List<Account> list = new ArrayList<Account>();

        // 6、处理结果，遍历rs中的所有数据
        // 光标向下移动一行，并且判断当前行是否有数据
        while (rs.next()) {
            Account account = new Account();

            // 获取数据 getXxx()
            int id = rs.getInt("id");
            String name = rs.getString("name");
            double money = rs.getDouble("money");

            // 赋值
            account.setId(id);
            account.setName(name);
            account.setMoney(money);

            // 存入集合
            list.add(account);
        }
        System.out.println(list);

        // 7、释放资源
        rs.close();
        stmt.close();
        conn.close();
    }
```

```java
@ToString
@Getter
@Setter
public class Account {
    private int id;
    private String name;
    private double money;
}
```

### 5.PreparedStatement

**PreparedStatement作用：**

1. 预编译SQL语句并执行：预防SQL注入问题

**SQL注入**

SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行攻击的方法

**实例**

```java
/**
     * 演示SQL注入
     */
    @Test
    public void testLogin_Inject() throws Exception {
        // 2、获取连接：如果连接的是本机mysql并且端口是默认的3306可以简化书写
        String url = "jdbc:mysql:///test";
        String username = "root";
        String password = "1234";
        Connection conn = DriverManager.getConnection(url, username, password);

        // 接收用户输入的用户名和密码
        String name = "zhangsan";
        String pwd = "' or '1' = '1";

        String sql = "select * from tb_user where username = '" + name + "' and password = '" + pwd + "'";

        // 获取statement对象
        Statement stmt = conn.createStatement();

        // 执行sql
        ResultSet rs = stmt.executeQuery(sql);

        // 判断登录是否成功
        if (rs.next()) {
            System.out.println("登录成功");
        } else {
            System.out.println("登录失败");
        }

        // 7、释放资源
        rs.close();
        stmt.close();
        conn.close();
    }
```

**步骤**

1. 获取PreparedStatement对象

   ```java
   // SQL语句中的参数值，使用?占位符替代
   String sql = "select * from user where username and password ?"
   
   // 通过Connection对象获取，并传入对应的sql语句
   PreparedStatement pstmt = conn.prepareStatement(sql);
   ```

2. 设置参数值
   PreparedStatement对象：`setXxx(参数1，参数2)`：给?赋值

   Xxx：数据类型；如setInt（参数1，参数2）参数：
   	参数1：？的位置编号，从1开始
   	参数2：？的值

3. 执行SQL

   `executeUpdate();/executeQuery();`：不需要再传递sql

**PreparedStatement好处：**

1. 预编译SQL,性能更高

   PreparedStatement预编译功能开启：`useServerPrepStmts=:true`

   配置MySQL执行日志(重启mysql服务后生效)

   ```
   log-output=FILE
   general-log=1
   general_log_file="D:\mysql.log"
   slow-query-log=1
   slow_query_log_file="D:\mysql_slow.log"
   long_query_time=2
   ```

2. 防止SQL注入：将敏感字符进行转义

**PreparedStatement原理：**

1. 在获取PreparedStatement对象时，将sql语句发送给mysql服务器进行检查，编译（这些步骤很耗时）
2. 执行时就不用再进行这些步骤了，速度更快
3. 如果sql模板一样，则只需要进行一次检查、编译

## 第四节 数据库连接池

### 1.简介

数据库连接池是个容器，负责分配、管理数据库连接（Connection）

它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个

释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏

**好处：**

1. 资源重用
2. 提升系统响应速度
3. 避免数据库连接遗漏

**数据库连接池实现**

标准接口：DataSource

官方（SUN）提供的数据库连接池标准接口，由第三方组织实现此接口

功能：获取连接

`Connection getConnection()`

常见的数据库连接池：

- DBCP
- C3PO
- Druid

### 2.Druid（德鲁伊）

- Druid连接池是阿里巴巴开源的数据库连接池项目
- 功能强大，性能优秀，是Java语言最好的数据库连接池之一

**Driud使用步骤**

1. 导入jar包 druid-1.1.12.jars
2. 定义配置文件
3. 加载配置文件
4. 获取数据库连接池对象
5. 获取连接

**代码**

```java
/**
 * Druid 数据库连接池演示
 */
public class DruidDemo {
    public static void main(String[] args) throws Exception {
        // 3、加载配置文件
        Properties prop = new Properties();
        prop.load(new FileInputStream("JDBC/src/druid.properties"));
        // 4、获取连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        // 5、获取数据库连接 Connection
        Connection connection = dataSource.getConnection();

        System.out.println(connection);
    }
}
```

## 第五节 练习

### 1.目的

完成商品品牌数据的增删改查操作

- 查询：查询所有数据
- 添加：添加品牌
- 修改：根据id修改
- 删除：根据id删除

### 2.准备环境

数据库表tb brand

实体类Brand

测试用例

### 3.查询所有

```java
/**
     * 查询所有
     * sql: select * from tb_brand;
     * 结果 List<Brand>
     */
    @Test
    public void testAll() throws Exception {
        // 1、获取Connection对象
        Properties prop = new Properties();
        prop.load(new FileInputStream("src/druid.properties"));

        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        Connection conn = dataSource.getConnection();

        // 2、定义sql语句
        String sql = "select * from tb_brand;";

        // 3、获取PreparedStatement对象
        PreparedStatement pstmt = conn.prepareStatement(sql);

        // 4、设置参数

        // 5、执行sql
        ResultSet rs = pstmt.executeQuery();

        // 6、处理结果 List<Brand> 封装Brand对象，装载List集合
        Brand brand = null;
        List<Brand> brands = new ArrayList<Brand>();
        while (rs.next()) {
            // 获取数据
            int id = rs.getInt("id");
            String brandName = rs.getString("brand_name");
            String companyName = rs.getString("company_name");
            int ordered = rs.getInt("ordered");
            String description = rs.getString("description");
            int status = rs.getInt("status");
            // 封装Brand对象
            brand = new Brand();
            brand.setId(id);
            brand.setBrandName(brandName);
            brand.setCompanyName(companyName);
            brand.setOrdered(ordered);
            brand.setDescription(description);
            brand.setStatus(status);

            // 装载集合
            brands.add(brand);
        }
        System.out.println(brands);

        // 7、释放资源
        rs.close();
        pstmt.close();
        conn.close();
    }
```

### 4.添加

```java
/**
     * 添加
     * sql: insert into tb_brand(brand_name,company_name,ordered,description,status) values(?,?,?,?,?);
     * 参数 除了id之外的所有参数信息
     * 结果 boolean
     */
    @Test
    public void testAdd() throws Exception {
        // 接收页面提交的参数
        String brandName = "香飘飘";
        String companyName = "香飘飘";
        int ordered = 1;
        String description = "绕地球一圈";
        int status = 1;

        // 1、获取Connection对象
        Properties prop = new Properties();
        prop.load(new FileInputStream("src/druid.properties"));

        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        Connection conn = dataSource.getConnection();

        // 2、定义sql语句
        String sql = "insert into tb_brand(brand_name,company_name,ordered,description,status) values(?,?,?,?,?);";

        // 3、获取PreparedStatement对象
        PreparedStatement pstmt = conn.prepareStatement(sql);

        // 4、设置参数
        pstmt.setString(1, brandName);
        pstmt.setString(2, companyName);
        pstmt.setInt(3, ordered);
        pstmt.setString(4, description);
        pstmt.setInt(5, status);

        // 5、执行sql
        int count = pstmt.executeUpdate();

        // 6、处理结果
        System.out.println(count > 0);

        // 7、释放资源
        pstmt.close();
        conn.close();
    }
```

### 5.修改

```java
/**
 * 修改
 * sql:
 * update tb_brand
 *  set brand_name=?,
 *  company_name = ?,
 *  ordered = ?,
 *  description = ?,
 *  status = ?
 *  where id =?
 * 参数 所有数据
 * 结果 boolean
 */
@Test
public void testUpdate() throws Exception {
    // 接收页面提交的参数
    String brandName = "香飘飘";
    String companyName = "香飘飘";
    int ordered = 1000;
    String description = "绕地球三圈";
    int status = 1;
    int id = 4;

    // 1、获取Connection对象
    Properties prop = new Properties();
    prop.load(new FileInputStream("src/druid.properties"));

    DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

    Connection conn = dataSource.getConnection();

    // 2、定义sql语句
    String sql = "update tb_brand\n" +
            "           set brand_name=?,\n" +
            "           company_name = ?,\n" +
            "           ordered = ?,\n" +
            "           description = ?,\n" +
            "           status = ?\n" +
            "       where id =?";

    // 3、获取PreparedStatement对象
    PreparedStatement pstmt = conn.prepareStatement(sql);

    // 4、设置参数
    pstmt.setString(1, brandName);
    pstmt.setString(2, companyName);
    pstmt.setInt(3, ordered);
    pstmt.setString(4, description);
    pstmt.setInt(5, status);
    pstmt.setInt(6, id);

    // 5、执行sql
    int count = pstmt.executeUpdate();

    // 6、处理结果
    System.out.println(count > 0);

    // 7、释放资源
    pstmt.close();
    conn.close();
}
```

### 6.删除

```               java
/**
 * 删除
 * sql:delete from tb brand where id = ?
 * 参数 id
 * 结果 boolean
 */
@Test
public void testDeleteById() throws Exception {
    // 接收页面提交的参数
    int id = 4;

    // 1、获取Connection对象
    Properties prop = new Properties();
    prop.load(new FileInputStream("src/druid.properties"));

    DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

    Connection conn = dataSource.getConnection();

    // 2、定义sql语句
    String sql = "delete from tb_brand where id = ?";

    // 3、获取PreparedStatement对象
    PreparedStatement pstmt = conn.prepareStatement(sql);

    // 4、设置参数
    pstmt.setInt(1, id);

    // 5、执行sql
    int count = pstmt.executeUpdate();

    // 6、处理结果
    System.out.println(count > 0);

    // 7、释放资源
    pstmt.close();
    conn.close();
}
```
