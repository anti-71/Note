---
名称: 线程同步 Lock 锁
章: 03 Java
节: "[[第一章 多线程]]"
tags:
  - 知识点
index: 8
---
##### Lock 锁

JDK5 开始提供，通过 `ReentrantLock` 实现加锁和解锁，更灵活、更强大

##### 常用方法

| 方法 | 说明 |
|:--|:--|
| `void lock()` | 获得锁 |
| `void unlock()` | 释放锁 |

##### 代码

```java
public class Account {
    private final Lock lk = new ReentrantLock();

    public void drawMoney(double money) {
        lk.lock();
        try {
            // 核心代码
        } finally {
            lk.unlock(); // 确保解锁
        }
    }
}
```
