---
名称: IO 框架 Commons-IO
章: 03 Java
节: "[[第四章 File-IO 流]]"
tags:
  - 知识点
index: 8
---
##### Commons-io 框架

Apache 提供的 IO 操作框架，提高开发效率

##### FileUtils 常用方法

| 方法 | 说明 |
|:--|:--|
| `copyFile(File src, File dest)` | 复制文件 |
| `copyDirectory(File src, File dest)` | 复制文件夹 |
| `deleteDirectory(File dir)` | 删除文件夹 |
| `readFileToString(File file, String encoding)` | 读数据 |
| `writeStringToFile(File file, String data, String charname, boolean append)` | 写数据 |

##### IOUtils 常用方法

| 方法 | 说明 |
|:--|:--|
| `copy(InputStream, OutputStream)` | 复制文件 |
| `copy(Reader, Writer)` | 复制文件 |
| `write(String data, OutputStream output, String charsetName)` | 写数据 |

##### 代码

```java
FileUtils.copyFile(new File("src.txt"), new File("dest.txt"));
FileUtils.deleteDirectory(new File("backup"));
```
