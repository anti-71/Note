---
名称: 字节流 FileOutputStream
章: 03 Java
节: "[[第四章 File-IO 流]]"
tags:
  - 知识点
index: 3
---
##### 作用

以内存为基准，将内存中的数据以字节形式写出到文件

##### 构造器

| 构造器 | 说明 |
|:--|:--|
| `FileOutputStream(File file)` | 覆盖模式 |
| `FileOutputStream(String path)` | 覆盖模式 |
| `FileOutputStream(File file, boolean append)` | 追加模式 |
| `FileOutputStream(String path, boolean append)` | 追加模式 |

##### 方法

| 方法 | 说明 |
|:--|:--|
| `write(int a)` | 写一个字节 |
| `write(byte[] buffer)` | 写一个字节数组 |
| `write(byte[] buffer, int pos, int len)` | 写字节数组的一部分 |

##### 代码

```java
OutputStream os = new FileOutputStream("file.txt", true);
os.write(97);
os.write('b');
os.write("\r\n".getBytes()); // 换行
os.write("我爱你中国".getBytes());
os.close();
```

##### 文件复制

字节流做复制一字不漏地转移全部字节，只要格式一致就没问题

```java
try (
    InputStream fis = new FileInputStream("source.png");
    OutputStream fos = new FileOutputStream("target.png");
) {
    byte[] buffer = new byte[1024];
    int len;
    while ((len = fis.read(buffer)) != -1) {
        fos.write(buffer, 0, len);
    }
}
```
