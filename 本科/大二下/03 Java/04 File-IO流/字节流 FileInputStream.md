---
名称: 字节流 FileInputStream
章: 03 Java
节: "[[第四章 File-IO 流]]"
tags:
  - 知识点
index: 2
---
##### 作用

以内存为基准，将磁盘文件中的数据以字节形式读入内存

##### 构造器

| 构造器 | 说明 |
|:--|:--|
| `FileInputStream(File file)` | 与源文件接通 |
| `FileInputStream(String pathname)` | 与源文件路径接通 |

##### 方法

| 方法 | 说明 |
|:--|:--|
| `read()` | 每次读取一个字节，返回 -1 表示无数据 |
| `read(byte[] buffer)` | 每次读取多个字节到数组 |
| `readAllBytes()` | 一次性读取全部字节 |

##### 代码

```java
// 每次读取一个字节（性能差，中文乱码）
InputStream is = new FileInputStream("file.txt");
int b;
while ((b = is.read()) != -1) {
    System.out.print((char) b);
}

// 每次读取多个字节（性能提升，但中文依然会乱码）
byte[] buffer = new byte[3];
int len;
while ((len = is.read(buffer)) != -1) {
    System.out.print(new String(buffer, 0, len));
}

// 一次性读取全部（避免乱码，但大文件可能内存溢出）
byte[] bytes = is.readAllBytes();
System.out.println(new String(bytes));
```
