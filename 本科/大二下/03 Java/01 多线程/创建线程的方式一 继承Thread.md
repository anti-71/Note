---
名称: 创建线程的方式一 继承 Thread
章: 03 Java
节: "[[第一章 多线程]]"
tags:
  - 知识点
index: 1
---
##### 步骤

1. 定义一个子类继承 `Thread`，重写 `run()` 方法
2. 创建子类对象
3. 调用 `start()` 启动线程

##### 优缺点

- 优点：编码简单
- 缺点：无法继承其他类，扩展性受限

##### 代码

```java
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程输出：" + i);
        }
    }
}

public class ThreadDemo1 {
    public static void main(String[] args) {
        Thread t1 = new MyThread();
        t1.start();
    }
}
```

##### 注意事项

- 必须调用 `start()` 而非 `run()`，直接调 `run()` 相当于单线程执行
- 不要将主线程任务放在启动子线程之前，否则主线程会先跑完
