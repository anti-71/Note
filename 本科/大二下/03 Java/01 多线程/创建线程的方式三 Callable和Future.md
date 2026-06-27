---
名称: 创建线程的方式三 Callable 和 Future
章: 03 Java
节: "[[第一章 多线程]]"
tags:
  - 知识点
index: 3
---
##### 问题背景

前两种方式重写的 `run()` 方法均不能直接返回结果

##### 步骤

1. 定义类实现 `Callable` 接口，重写 `call()` 方法
2. 将 `Callable` 对象封装成 `FutureTask`（线程任务对象）
3. 将 `FutureTask` 交给 `Thread` 对象
4. 调用 `start()` 启动线程
5. 通过 `FutureTask.get()` 获取线程执行结果

##### 优缺点

- **优点**：扩展性强，**可获取线程执行结果**
- **缺点**：编码复杂一点

##### 代码

```java
class MyCallable implements Callable<String> {
    private int n;
    public MyCallable(int n) { this.n = n; }

    public String call() {
        int sum = 0;
        for (int i = 0; i <= n; i++) sum += i;
        return "结果：" + sum;
    }
}

public class ThreadDemo3 {
    public static void main(String[] args) throws Exception {
        Callable<String> c1 = new MyCallable(100);
        FutureTask<String> f1 = new FutureTask<>(c1);
        new Thread(f1).start();
        System.out.println(f1.get()); // 获取结果
    }
}
```
