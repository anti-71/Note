---
名称: 创建线程的方式二 实现 Runnable
章: 03 Java
节: "[[第一章 多线程]]"
tags:
  - 知识点
index: 2
---
##### 步骤

1. 定义类实现 `Runnable` 接口，重写 `run()`
2. 创建任务对象
3. 将任务对象交给 `Thread` 处理
4. 调用 `start()` 启动线程

##### 优缺点

- 优点：可继续继承其他类、实现其他接口，扩展性强
- 缺点：需要多一个 `Runnable` 对象

##### 代码

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程输出：" + i);
        }
    }
}

public class ThreadDemo2 {
    public static void main(String[] args) {
        Runnable r = new MyRunnable();
        Thread t1 = new Thread(r);
        t1.start();
    }
}
```

##### 匿名内部类写法

```java
new Thread(new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程输出：" + i);
        }
    }
}).start();

// Lambda 简化
new Thread(() -> {
    for (int i = 0; i < 5; i++) {
        System.out.println("子线程输出：" + i);
    }
}).start();
```
