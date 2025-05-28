# File-IO流

## 第四节 IO流

### 1.认识IO流

l指Input,称为输入流：负责把数据读到内存中去

O指Output,称为输出流：负责写数据出去

**IO**流的分类

1. 按流方向，分为：输入流、输出流
2. 按流内容，分为：字节流（适合操作所有类型的文件）、字符流（只适合操作纯文本文件）

**IO流的体系**

- 字节输入流
- 字节输出流
- 字符输入流
- 字符输出流

### 2.字节流

**FileInputstream（文件字节输入流）**

**作用**：以内存为基准，可以把磁盘文件中的数据以字节的形式读入到内存中去

| 构造器                                    | 说明                           |
| ----------------------------------------- | ------------------------------ |
| `public FileInputStream(File file)`       | 创建字节输入流管道与源文件接通 |
| `public FileInputStream(String pathname)` | 创建字节输入流管道与源文件接通 |

| 方法名称                         | 说明                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| `public int read()`              | 每次读取一个字节返回，如果发现没有数据可读会返回 -1          |
| `public int read(byte[] buffer)` | 每次用一个字节数组去读取数据，返回字节数组读取了多少个字节，如果发现没有数据可读会返回 - 1 |

**代码**

```java
// 目标：掌握文件字节输入流读取文件中的字节数组到内存中来
        // 创建文件字节输入流管道与源文件接通
//        InputStream is = new FileInputStream(new File("File-IO\\src\\jeunesse02.txt"));
        InputStream is = new FileInputStream("File-IO\\src\\jeunesse03.txt"); // 简化写法

        // 开始读取文件中的字节并输出：每次读取一个字节
        // 定义一个变量记住每次读取的一个字节
        int b;
        while ((b = is.read()) != -1) {
            System.out.print((char) b);
        }
        // 每次读取一个字节的问题：性能较差，读取汉字输出一定会乱码！
```



**注意事项**

- 使用FilelnputStream每次读取一个字节，读取性能较差，并且读取汉字输出会乱码
- 使用FilelnputStream每次读取多个字节，读取性能得到了提升，但读取汉字输出还是会乱码

1、使用字节流读取中文，如何保证输出不乱码，怎么解决？

定义一个与文件一样大的字节数组，一次性读取完文件的全部字节

**代码**

```java
// 目标：掌握文件字节输入流读取文件中的字节数组到内存中来
        // 创建文件字节输入流管道与源文件接通
//        InputStream is = new FileInputStream(new File("File-IO\\src\\jeunesse02.txt"));
        InputStream is = new FileInputStream("File-IO\\src\\jeunesse03.txt"); // 简化写法

        // 开始读取文件中的字节并输出：每次读取多个字节
        // 定义一个字节数组用于每次读取字节
        byte[] buffer = new byte[3];
        // 定义一个变量记住每次读取了多少个字节
        int len;
        while ((len = is.read(buffer)) != -1) {
            // 把读取到的字节数组转换成字符串输出，读取多少倒出多少
            String str = new String(buffer, 0, len);
            System.out.print(str);
        }
        // 拓展：每次读取多个字节，性能得到提升，因为每次读取多个字节，可以减少硬盘和内存的交互次数，从而提升性能
        // 依然无法避免读取汉字输出乱码的问题：存在截断汉字字节的可能性！
```

**一次读取完全部字节**

Java官方为InputStream提供了如下方法，可以直接把文件的全部字节读取到一个字节数组中返回

| 方法名称                                          | 说明                                                         |
| ------------------------------------------------- | ------------------------------------------------------------ |
| `public byte[] readAllBytes() throws IOException` | 直接将当前字节输入流对应的文件对象的字节数据装到一个字节数组返回 |

2、直接把文件数据全部读取到一个字节数组可以避免乱码，是否存在问题？

如果文件过大，创建的字节数组也会过大，可能引起内存溢出

读取文本适合用字符流；字节流适合做数据的转移，比如：文件复制

**代码**

```java
// 目标：掌握文件字节输入流读取文件中的字节数组到内存中来
        // 1、创建文件字节输入流管道与源文件接通
//        InputStream is = new FileInputStream(new File("File-IO\\src\\jeunesse02.txt"));
        InputStream is = new FileInputStream("File-IO\\src\\jeunesse04.txt"); // 简化写法

        // 2、一次性读完文件的全部字节
        byte[] bytes = is.readAllBytes();

        System.out.println(new String(bytes));
```

**FileOutputStream（文件字节输出流）**

**作用**：以内存为基准，把内存中的数据以字节的形式写出到文件中去

| 构造器                                                     | 说明                                           |
| ---------------------------------------------------------- | ---------------------------------------------- |
| `public FileOutputStream(File file)`                       | 创建字节输出流管道与源文件对象接通             |
| `public FileOutputStream(String filepath)`                 | 创建字节输出流管道与源文件路径接通             |
| `public FileOutputStream(File file, boolean append)`       | 创建字节输出流管道与源文件对象接通，可追加数据 |
| `public FileOutputStream(String filepath, boolean append)` | 创建字节输出流管道与源文件路径接通，可追加数据 |

| 方法名称                                             | 说明                       |
| ---------------------------------------------------- | -------------------------- |
| `public void write(int a)`                           | 写一个字节出去             |
| `public void write(byte[] buffer)`                   | 写一个字节数组出去         |
| `public void write(byte[] buffer, int pos, int len)` | 写一个字节数组的一部分出去 |
| `public void close() throws IOException`             | 关闭流                     |

**代码**

```java
// 目标：学会使用文件字节输出流
        // 1、创建一个文件字节输出流管道，与目标文件接通
        OutputStream os = new FileOutputStream("File-IO\\src\\jeunesse05.txt",true);

        // 2、写入数据
        os.write(97);
        os.write('b');
//        os.write('徐'); // 会乱码
        os.write("\r\n".getBytes()); // 换行

        // 3、写一个字节数组出去
        byte[] bytes = "我爱你中国666".getBytes();
        os.write(bytes);
        os.write("\r\n".getBytes()); // 换行

        // 4、写一个字节数组的某一部分出去
        os.write(bytes, 0, 3);
        os.write("\r\n".getBytes()); // 换行

        os.close(); // 关闭管道 释放资源
```

**文件复制**

任何文件的底层都是字节，字节流做复制，是一字不漏的转移完全部字节，只要复制后的文件格式一致就没问题！

**代码**

```java
public static void main(String[] args) {
        // 目标：使用字节流完成文件的复制操作
        // 源文件：D:\Note\Java\Assets\HTTP协议.png
        // 目标文件：D:\Ace\Java\Java_Study\File-IO\src\HTTP协议.png
        try {
            copyFile("D:\\Note\\Java\\Assets\\HTTP协议.png", "D:\\Ace\\Java\\Java_Study\\File-IO\\src\\HTTP协议.png");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static void copyFile(String s, String s1) throws Exception {
        // 1、创建字节输入流管道与源文件接通
        InputStream fis = new FileInputStream(s);
        OutputStream fos = new FileOutputStream(s1);
        // 2、读取一个字节数组，写入一个字节数组 1024 * 1024 + 3
        byte[] buffer = new byte[1024];
        int len;
        while ((len = fis.read(buffer)) != -1) {
            fos.write(buffer, 0, len); // 读取多少个字节，就写入多少个字节
        }
        System.out.println("复制完成！");
        // 3、关闭流
        fos.close();
        fis.close();
    }
```

**资源释放问题**

try-catch-finally

finally代码区的特点：无论try中的程序是正常执行了，还是出现了异常，最后都一定会执行finally区，除非VM终止

**作用**：一般用于在程序执行完成后进行资源的释放操作（专业级做法）

**代码**

```java
InputStream is null;
OutputStream os null;
try {
    ...
} catch (Exception e) {
	e.printstackTrace();
} finally {
    //关闭资源！
	try {
		if(os != null) os.close();
    } catch (Exception e) {
		e.printStackTrace();
	}
	try {
    	if(is != null) is.close();
	} catch (Exception e) {
		e.printStackTrace();
    }
}
```

try-with-resource

JDK 7开始提供了更简单的资源释放方案：try-with-resource

该资源使用完毕后，会自动调用其close()方法，完成对资源的释放！

()中只能放置资源，否则报错

**什么是资源呢？**

资源一般指的是最终实现了AutoCloseable接口

**代码**

```java
public static void main(String[] args) {
        // 目标：掌握资源的新方式：try-with-resource
        // 源文件：D:\Note\Java\Assets\HTTP协议.png
        // 目标文件：D:\Ace\Java\Java_Study\File-IO\src\HTTP协议.png
        copyFile("D:\\Note\\Java\\Assets\\HTTP协议.png", "D:\\Ace\\Java\\Java_Study\\File-IO\\src\\HTTP协议.png");
    }

    private static void copyFile(String s, String s1) {
        // 1、创建字节输入流管道与源文件接通
        try (
                // 这里只能放置资源对象，用完后，最终会自动调用其cL0se方法关闭！！
                InputStream fis = new FileInputStream(s);
                OutputStream fos = new FileOutputStream(s1);
                ){
            // 2、读取一个字节数组，写入一个字节数组 1024 * 1024 + 3
            byte[] buffer = new byte[1024];
            int len;
            while ((len = fis.read(buffer)) != -1) {
                fos.write(buffer, 0, len); // 读取多少个字节，就写入多少个字节
            }
            System.out.println("复制完成！");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

### 3.字符流

**FileReader（文件字符输入流）**

**作用**：以内存为基准，可以把文件中的数据以字符的形式读入到内存中去

| 构造器                               | 说明                           |
| ------------------------------------ | ------------------------------ |
| `public FileReader(File file)`       | 创建字符输入流管道与源文件接通 |
| `public FileReader(String pathname)` | 创建字符输入流管道与源文件接通 |

| 方法名称                         | 说明                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| `public int read()`              | 每次读取一个字符返回，如果发现没有数据可读会返回 - 1         |
| `public int read(char[] buffer)` | 每次用一个字符数组去读取数据，返回字符数组读取了多少个字符，如果发现没有数据可读会返回 - 1 |

**代码**

```java
// 目标：掌握文件字符输入流读取字符内容到程序中来
        try (
                // 1、创建文件字符输入流与源文件接通
                Reader fr = new FileReader("File-IO\\src\\jeunesse06.txt")
        ) {
            // 2、定义一个字符数组，每次读多个字符
            char[] buffer = new char[3];
            int len; // 用于记录每次读取了多少个字符
            while ((len = fr.read(buffer)) != -1) {
                // 3、将读取的字符数组转换成字符串并输出
                System.out.print(new String(buffer, 0, len));
            }
            // 拓展：文件字符输入流每次读取多个字符，性能较好，而且读取中文
            // 是按照字符读取，不会出现乱码！这是一种读取中文很好的方案
        } catch (Exception e) {
            e.printStackTrace();
        }
```

**FileWriter（文件字符输出流）**

**作用**：以内存为基准，把内存中的数据以字符的形式写出到文件中去

| 构造器                                               | 说明                                           |
| ---------------------------------------------------- | ---------------------------------------------- |
| `public FileWriter(File file)`                       | 创建字节输出流管道与源文件对象接通             |
| `public FileWriter(String filepath)`                 | 创建字节输出流管道与源文件路径接通             |
| `public FileWriter(File file, boolean append)`       | 创建字节输出流管道与源文件对象接通，可追加数据 |
| `public FileWriter(String filepath, boolean append)` | 创建字节输出流管道与源文件路径接通，可追加数据 |

| 方法名称                                    | 说明                 |
| ------------------------------------------- | -------------------- |
| `void write(int c)`                         | 写一个字符           |
| `void write(String str)`                    | 写一个字符串         |
| `void write(String str, int off, int len)`  | 写一个字符串的一部分 |
| `void write(char[] cbuf)`                   | 写入一个字符数组     |
| `void write(char[] cbuf, int off, int len)` | 写入字符数组的一部分 |

**代码**

```java
// 目标：搞清楚文件字符输出流的使用：写字符出去的流
        try (
                // 1、创建一个字符输出流对象，指定写出的目的地
//                Writer fw = new FileWriter("File-IO\\src\\jeunesse07-out.txt") // 覆盖管道
                Writer fw = new FileWriter("File-IO\\src\\jeunesse07-out.txt",true) // 追加数据
        ) {
            // 2、写一个字符出去
            fw.write('a');
            fw.write(98);
            fw.write('施');
            fw.write("\r\n"); // 换行

            // 3、写一个字符出去
            fw.write("java");
            fw.write("我爱Java，虽然Java不是最好的编程语言，但是可以挣钱");
            fw.write("\r\n"); // 换行

            // 4、写字符串的一部分出去
            fw.write("java", 0, 2);
            fw.write("\r\n"); // 换行

            // 5、写一个字符数组出去
            char[] chs = "java".toCharArray();
            fw.write(chs);
            fw.write("\r\n"); // 换行

            // 6、写一个字符数组的某部分出去
            fw.write(chs, 0, 2);
            fw.write("\r\n"); // 换行
        } catch (Exception e) {
            e.printStackTrace();
        }
```

**字符输出流的注意实现**

字符输出流写出数据后，必须刷新流，或者关闭流，写出去的数据才能生效

| 方法名称                                 | 说明                                                 |
| ---------------------------------------- | ---------------------------------------------------- |
| `public void flush() throws IOException` | 刷新流，就是将内存中缓存的数据立即写到文件中去生效！ |
| `public void close() throws IOException` | 关闭流的操作，包含了刷新！                           |

**代码**

```java
// fw.flush(); // 刷新缓冲区，将缓冲区中的数据全部写出去
            // 刷新后，流可以继续使用
            // fw.close(); // 关闭资源，关闭包含了刷新！关闭后流不能继续使用
```

### 4.缓冲流

**BufferedInputstream（缓冲字节输入流）**

**作用**：可以提高字节输入流读取数据的性能

**原理**：缓冲字节输入流自带了8KB缓冲池；缓冲字节输出流也自带了8KB缓冲池

| 构造器                                         | 说明                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| `public BufferedInputStream(InputStream is)`   | 把低级的字节输入流包装成一个高级的缓冲字节输入流，从而提高读数据的性能 |
| `public BufferedOutputStream(OutputStream os)` | 把低级的字节输出流包装成一个高级的缓冲字节输出流，从而提高写数据的性能 |

**代码**

```java
// 目标：掌握缓冲字节流的使用
        // 源文件：D:\Note\Java\Assets\HTTP协议.png
        // 目标文件：D:\Ace\Java\Java_Study\File-IO\src\HTTP协议.png
        copyFile("D:\\Note\\Java\\Assets\\HTTP协议.png", "D:\\Ace\\Java\\Java_Study\\File-IO\\src\\HTTP协议.png");
    }

    private static void copyFile(String s, String s1) {
        // 1、创建字节输入流管道与源文件接通
        try (
                // 这里只能放置资源对象，用完后，最终会自动调用其close方法关闭！！
                InputStream fis = new FileInputStream(s);
                // 把低级的字节输入流包装成高级的缓冲字节输入流
                InputStream bis = new BufferedInputStream(fis);

                OutputStream fos = new FileOutputStream(s1);
                // 把低级的字节输出流包装成高级的缓冲字节输出流
                OutputStream bos = new BufferedOutputStream(fos);
                ){
            // 2、读取一个字节数组，写入一个字节数组 1024 * 1024 + 3
            byte[] buffer = new byte[1024];
            int len;
            while ((len = bis.read(buffer)) != -1) {
                bos.write(buffer, 0, len); // 读取多少个字节，就写入多少个字节
            }
            System.out.println("复制完成！");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

**BufferedReader（缓冲字符输入流）**

**作用**：自带8K（8192）的字符缓冲池，可以提高字符输入流读取字符数据的性能

| 构造器                            | 说明                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| `public BufferedReader(Reader r)` | 把低级的字符输入流包装成字符缓冲输入流管道，从而提高字符输入流读字符数据的性能 |

字符缓冲输入流新增的功能：按照行读取字符

| 方法                       | 说明                                              |
| -------------------------- | ------------------------------------------------- |
| `public String readLine()` | 读取一行数据返回，如果没有数据可读了，会返回 null |

**代码**

```java
// 目标：搞清楚缓冲字符输入流读取字符内容：性能提升了，多了按照行读取文本的能力
        try (
                // 1、创建文件字符输入流与源文件接通
                Reader fr = new FileReader("File-IO\\src\\jeunesse08.txt");
                // 2、创建缓冲字符输入流包装低级的字符输入流
                BufferedReader br = new BufferedReader(fr);
        ) {
//            System.out.println(br.readLine());
//            System.out.println(br.readLine());
//            System.out.println(br.readLine());
//            System.out.println(br.readLine());
//            System.out.println(br.readLine());
//            System.out.println(br.readLine()); // null

            // 使用循环改进，来按照行读取数据
            // 定义一个字符串变量用于记住每次读取的一行数据
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
            // 目前读取文本最优雅的方案：性能好，不乱码，可以按照行读取
        } catch (Exception e) {
            e.printStackTrace();
        }
```

**BufferedWriter（缓冲字符输出流）**

**作用**：自带8K的字符缓冲池，可以提高字符输出流写字符数据的性能

| 构造器                            | 说明                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| `public BufferedWriter(Writer r)` | 把低级的字符输出流包装成一个高级的缓冲字符输出流管道，从而提高字符输出流写数据的性能 |

字符缓冲输出流新增的功能：换行

| 方法                    | 说明 |
| ----------------------- | ---- |
| `public void newLine()` | 换行 |

**代码**

```java
// 目标：搞清楚缓冲字符输出流的使用：提升了字符输出流的写字符的性能，多了换行功能
        try (
                // 1、创建一个字符输出流对象，指定写出的目的地
//                Writer fw = new FileWriter("File-IO\\src\\jeunesse07-out.txt") // 覆盖管道
                Writer fw = new FileWriter("File-IO\\src\\jeunesse07-out.txt",true); // 追加数据
                // 2、创建一个缓冲字符输出流对象，把字符输出流对象作为构造参数传递给缓冲字符输出流对象
                BufferedWriter bw = new BufferedWriter(fw);
        ) {
            // 2、写一个字符出去
            bw.write('a');
            bw.write(98);
            bw.write('施');
            bw.newLine(); // 换行

            // 3、写一个字符出去
            bw.write("java");
            bw.write("我爱Java，虽然Java不是最好的编程语言，但是可以挣钱");
            bw.newLine(); // 换行

            // 4、写字符串的一部分出去
            bw.write("java", 0, 2);
            bw.newLine(); // 换行

            // 5、写一个字符数组出去
            char[] chs = "java".toCharArray();
            bw.write(chs);
            bw.newLine(); // 换行

            // 6、写一个字符数组的某部分出去
            bw.write(chs, 0, 2);
            bw.newLine(); // 换行

            // bw.flush(); // 刷新缓冲区，将缓冲区中的数据全部写出去
            // 刷新后，流可以继续使用
            // bw.close(); // 关闭资源，关闭包含了刷新！关闭后流不能继续使用
        } catch (Exception e) {
            e.printStackTrace();
        }
```

**缓冲流的案例**

**需求**：把《出师表》的文章顺序进行恢复到一个新文件中

**分析**：

1. 定义一个缓存字符输入流管道与源文件接通
2. 定义一个List集合存储读取的每行数据
3. 定义一个循环按照行读取数据，存入到List集合中去
4. 对List集合中的每行数据按照首字符编号升序排序
5. 定义一个缓存字符输出管道与目标文件接通
6. 遍历List集合中的每个元素，用缓冲输出管道写出并换行

**代码**

```java
package com.jeunesse.demo11buffedWrite;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class BufferedTest2 {
    public static void main(String[] args) {
        // 目标：完成出师表的案例
        try (
                // 1、创建一个字符缓冲输入流对象与源文件链接
                BufferedReader br = new BufferedReader(new FileReader("File-IO\\src\\csb.txt"));
                // 6、创建一个字符缓冲输出流对象与目标文件链接
                BufferedWriter bw = new BufferedWriter(new FileWriter("File-IO\\src\\csb_out.txt"));
        ) {
            // 2、提前准备一个List集合存储每段内容
            List<String> data = new ArrayList<>();

            // 3、按照行读取数据，存入到data集合中去
            String line;
            while ((line = br.readLine()) != null) {
                data.add(line);
            }

            // 4、给集合中的每段内容，按照首字符排序
            Collections.sort(data);
            System.out.println(data);

            // 5、遍历集合，将每段内容写入到目标文件中
            for (String s : data) {
                bw.write(s);
                bw.newLine(); // 换行
            }
            System.out.println("出师表生成完毕！");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 5.性能分析

**原始流、缓冲流的性能分析**

**测试用例**：

分别使用原始的字节流，以及字节缓冲流复制一个很大视频

**测试步骤**：

1. 使用低级的字节流按照一个一个字节的形式复制文件
2. 使用低级的字节流按照字节数组的形式复制文件
3. 使用高级的缓冲字节流按照一个一个字节的形式复制文件
4. 使用高级的缓冲字节流按照字节数组的形式复制文件

```java
package com.jeunesse.demo11buffedWrite;

import java.io.*;

public class TimeTest3 {
    private static final String SRC_PATH = "File-IO/src/13-Stream流-常用终结方法.mp4";
    private static final String DEST_PATH = "File-IO/src/";

    public static void main(String[] args) {
        // 目标：缓冲流，低级流的性能分析
        // 使用低级的字节流按照一个一个字节的形式复制文件：非常的慢，简直让人无法忍受，直接淘汰，禁止使用！！
//        copyFile1();
        // 使用低级的字节流按照字节数组的形式复制文件：是比较慢的，还可以接受
        copyFile2();
        // 使用高级的缓冲字节流按照一个一个字节的形式复制文件：虽然是高级管道，但一个一个字节的复制还是太慢了，无法忍受，直接淘汰！
//        copyFile3();
        // 使用高级的缓冲字节流按照字节数组的形式复制文件：非常快！推荐使用！
        copyFile4();
    }

    // 使用高级的缓冲字节流按照字节数组的形式复制文件
    private static void copyFile4() {
        long start = System.currentTimeMillis();
        try(
                InputStream fis = new FileInputStream(SRC_PATH);
                OutputStream fos = new FileOutputStream(DEST_PATH + "4.mp4");
                BufferedInputStream bis = new BufferedInputStream(fis);
                BufferedOutputStream bos = new BufferedOutputStream(fos);
                ) {
            byte[] buffer = new byte[1024 * 8]; // 1KB
            int len;
            while ((len = bis.read(buffer)) != -1) {
                bos.write(buffer, 0, len);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        long end = System.currentTimeMillis();
        System.out.println("高级缓冲字节流按照字节数组的形式复制文件耗时：" + (end - start) / 1000.0 + "s");
    }

    // 使用高级的缓冲字节流按照一个一个字节的形式复制文件
    private static void copyFile3() {
        long start = System.currentTimeMillis();
        try(
                InputStream fis = new FileInputStream(SRC_PATH);
                OutputStream fos = new FileOutputStream(DEST_PATH + "3.mp4");
                BufferedInputStream bis = new BufferedInputStream(fis);
                BufferedOutputStream bos = new BufferedOutputStream(fos);
                ) {
            int b;
            while ((b = bis.read()) != -1) {
                bos.write(b);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        long end = System.currentTimeMillis();
        System.out.println("高级缓冲字节流按照一个一个字节的形式复制文件耗时：" + (end - start) / 1000.0 + "s");
    }

    // 使用低级的字节流按照字节数组的形式复制文件
    private static void copyFile2() {
        long start = System.currentTimeMillis();
        try(
                InputStream fis = new FileInputStream(SRC_PATH);
                OutputStream fos = new FileOutputStream(DEST_PATH + "2.mp4");
                ) {
            byte[] buffer = new byte[1024 * 8]; // 1KB
            int len;
            while ((len = fis.read(buffer)) != -1) {
                fos.write(buffer, 0, len);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        long end = System.currentTimeMillis();
        System.out.println("低级字节流按照字节数组的形式复制文件耗时：" + (end - start) / 1000.0 + "s");
    }

    // 使用低级的字节流按照一个一个字节的形式复制文件
    public static void copyFile1() {
        // 拿系统当前时间
        long start = System.currentTimeMillis(); // 获取当前时间毫秒数，从1970-1-100:00:00开始走到此刻的。总毫秒值1s=1000ms
        try(
                InputStream fis = new FileInputStream(SRC_PATH);
                OutputStream fos = new FileOutputStream(DEST_PATH + "1.mp4");
                ) {
            int b;
            while ((b = fis.read()) != -1) {
                fos.write(b);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        long end = System.currentTimeMillis(); // 获取当前时间毫秒数，从1970-1-100:00:00开始走到此刻的。总毫秒值1s=1000ms
        System.out.println("低级字节流按照一个一个字节的形式复制文件耗时：" + (end - start) / 1000.0 + "s");
    }
}
```

### 6.其他流

**InputStreamReader（字符输入转换流）**

解决不同编码时，字符流读取文本内容乱码的问题

**解决思路**：先获取文件的原始字节流，再将其按真实的字符集编码转成字符输入流，这样字符输入流中的字符就不乱码了

| 构造器                                                     | 说明                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| `public InputStreamReader(InputStream is)`                 | 把原始的字节输入流，按照代码默认编码转成字符输入流（与直接用 FileReader 的效果一样） |
| `public InputStreamReader(InputStream is, String charset)` | 把原始的字节输入流，按照指定字符集编码转成字符输入流 (重点)  |

**代码**

```java
// 目标：使用字符输入转换流InputStreamReader解决不同编码读取乱码的问题
// 代码：UTF-8 文件UTF-8 读取不乱码
// 代码：UTF-8 文件GBK 读取乱码
try (
        // 先提取文件的原始字节流
        InputStream is = new FileInputStream("File-IO\\src\\jeunesse09.txt");
        // 指定字符集把原始字节流转换成字符输入流
        Reader isr = new InputStreamReader(is, "GBK");
        // 2、创建缓冲字符输入流包装低级的字符输入流
        BufferedReader br = new BufferedReader(isr);
) {
    // 定义一个字符串变量用于记住每次读取的一行数据
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
    // 目前读取文本最优雅的方案：性能好，不乱码，可以按照行读取
} catch (Exception e) {
    e.printStackTrace();
}
```

**PrintStream/PrintWriter（打印流）**

**作用**：打印流可以实现更方便、更高效的打印数据出去，能实现打印啥出去就是啥出去

**PrintStream提供的打印数据的方案**

| 构造器                                                       | 说明                                       |
| ------------------------------------------------------------ | ------------------------------------------ |
| `public PrintStream(OutputStream/File/String)`               | 打印流直接通向字节输出流 / 文件 / 文件路径 |
| `public PrintStream(String fileName, Charset charset)`       | 可以指定写出去的字符编码                   |
| `public PrintStream(OutputStream out, boolean autoFlush)`    | 可以指定实现自动刷新                       |
| `public PrintStream(OutputStream out, boolean autoFlush, String encoding)` | 可以指定实现自动刷新，并可指定字符的编码   |

| 方法                                         | 说明                   |
| -------------------------------------------- | ---------------------- |
| `public void println(Xxx xx)`                | 打印任意类型的数据出去 |
| `public void write(int/byte[]/byte[]一部分)` | 可以支持写字节数据出去 |

**代码**

```java
 // 目标：打印流的使用
        try (
                PrintStream  ps = new PrintStream("File-IO\\src\\ps.txt");
                ) {
            ps.println(97);
            ps.println('a');
            ps.println(true);
            ps.println(99.99);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

**DataOutputStream（数据输出流）**

允许把数据和其类型一并写出去

| 构造器                                      | 说明                                 |
| ------------------------------------------- | ------------------------------------ |
| `public DataOutputStream(OutputStream out)` | 创建新数据输出流包装基础的字节输出流 |

| 方法                                                         | 说明                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| `public final void writeByte(int v) throws IOException`      | 将 byte 类型的数据写入基础的字节输出流                |
| `public final void writeInt(int v) throws IOException`       | 将 int 类型的数据写入基础的字节输出流                 |
| `public final void writeDouble(Double v) throws IOException` | 将 double 类型的数据写入基础的字节输出流              |
| `public final void writeUTF(String str) throws IOException`  | 将字符串数据以 UTF - 8 编码成字节写入基础的字节输出流 |
| `public void write(int/byte[]/byte[]一部分)`                 | 支持写字节数据出去                                    |

**代码**

```java
// 目标：特殊数据流的使用
        try (
                DataOutputStream  dos = new DataOutputStream(new FileOutputStream("File-IO/src/data.txt"));
                ) {
            dos.writeByte(34);
            dos.writeUTF("你好");
            dos.writeInt(100);
            dos.writeDouble(3.14);
        } catch (Exception e) {
            e.printStackTrace();
        }
// 目标：特殊数据流的使用
        try (
                DataInputStream dis = new DataInputStream(new FileInputStream("File-IO/src/data.txt"));
                ) {
            System.out.println(dis.readByte());
            System.out.println(dis.readUTF());
            System.out.println(dis.readInt());
            System.out.println(dis.readDouble());
        } catch (Exception e) {
            e.printStackTrace();
        }
```

### 7.IO框架

**Commons-io框架**

Commons-io是apache开源基金组织提供的一组有关I0操作的小框架，目的是提高I0流的开发效率

| FileUtils 类提供的部分方法展示                               | 说明       |
| ------------------------------------------------------------ | ---------- |
| `public static void copyFile(File srcFile, File destFile)`   | 复制文件   |
| `public static void copyDirectory(File srcDir, File destDir)` | 复制文件夹 |
| `public static void deleteDirectory(File directory)`         | 删除文件夹 |
| `public static String readFileToString(File file, String encoding)` | 读数据     |
| `public static void writeStringToFile(File file, String data, String charname, boolean append)` | 写数据     |

| IOUtils 类提供的部分方法展示                                 | 说明     |
| ------------------------------------------------------------ | -------- |
| `public static int copy(InputStream inputStream, OutputStream outputStream)` | 复制文件 |
| `public static int copy(Reader reader, Writer writer)`       | 复制文件 |
| `public static void write(String data, OutputStream output, String charsetName)` | 写数据   |

**代码**

```java
// 目标：IO框架
        FileUtils.copyFile(new File("File-IO\\src\\csb_out.txt"),new File("File-IO\\src\\csb_out2.txt"));
        FileUtils.deleteDirectory(new File("File-IO\\src\\com\\jeunesse\\demo4fileinputStream-副本"));
```

## 第五节 综合小案例

**完善石头迷阵游戏中的历史最少步骤信息展示**

**需求**

要求在石头迷阵游戏中，增加一个游戏胜利后的最少步数量记录和展示功能，展示位置如图所示

**分析**

- 最小步数这个数据是需要长久保存的所以可以考虑用IO流把这个数据写出到一个磁盘文件保存起来
- 每次启动游戏要读取这个最小步数展示出来
- 玩家胜利后，需要判断是否比文件里记录的历史最小步数少，如果更少就要更新文件里的这个最少步数数据