---
名称: JDBC 练习 CRUD
章: 03 Java
节: "[[第五章 JDBC]]"
tags:
  - 知识点
index: 9
---
##### 查询所有

```java
String sql = "select * from tb_brand;";
PreparedStatement pstmt = conn.prepareStatement(sql);
ResultSet rs = pstmt.executeQuery();

List<Brand> brands = new ArrayList<>();
while (rs.next()) {
    Brand brand = new Brand();
    brand.setId(rs.getInt("id"));
    brand.setBrandName(rs.getString("brand_name"));
    brand.setCompanyName(rs.getString("company_name"));
    brand.setOrdered(rs.getInt("ordered"));
    brand.setDescription(rs.getString("description"));
    brand.setStatus(rs.getInt("status"));
    brands.add(brand);
}
```

##### 添加

```java
String sql = "insert into tb_brand(brand_name,company_name,ordered,description,status) values(?,?,?,?,?)";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, brandName);
pstmt.setString(2, companyName);
pstmt.setInt(3, ordered);
pstmt.setString(4, description);
pstmt.setInt(5, status);
int count = pstmt.executeUpdate();
System.out.println(count > 0);
```

##### 修改

```java
String sql = "update tb_brand set brand_name=?, company_name=?, ordered=?, description=?, status=? where id=?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, brandName);
// ...
pstmt.setInt(6, id);
int count = pstmt.executeUpdate();
```

##### 删除

```java
String sql = "delete from tb_brand where id = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setInt(1, id);
int count = pstmt.executeUpdate();
System.out.println(count > 0);
```
