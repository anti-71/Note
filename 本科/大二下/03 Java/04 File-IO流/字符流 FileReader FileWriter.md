---
名称: 字符流 FileReader FileWriter
章: 03 Java
节: "[[第四章 File-IO 流]]"
tags:
  - 知识点
index: 5
---
##### FileReader（文件字符输入流）

以字符形式读取文件，适合纯文本文件，中文不乱码

```java
try (Reader fr = new FileReader("file.txt")) {
    char[] buffer = new char[3];
    int len;
    while ((len = fr.read(buffer)) != -1) {
        System.out.print(new String(buffer, 0, len));
    }
}
```

##### FileWriter（文件字符输出流）

以字符形式写出数据到文件

```java
try (Writer fw = new FileWriter("file.txt", true)) {
    fw.write('a');
    fw.write("我爱Java");
    fw.write("\r\n");
}
```

> [!note]
> 字符输出流写出数据后，必须 `flush()` 或 `close()` 才能生效

| 方法 | 说明 |
|:--|:--|
| `flush()` | 刷新缓存，流可继续使用 |
| `close()` | 关闭流，包含刷新操作 |
