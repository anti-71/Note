---
名称: GUI 编程快速入门
章: 03 Java
节: "[[第二章 面向对象编程高级]]"
tags:
  - 知识点
index: 1
---
##### 什么是 GUI 编程

GUI（Graphical User Interface）指图形用户界面，通过窗口、按钮、文本框等图形元素与用户交互

##### Java GUI 编程包

- **AWT**（Abstract Window Toolkit）：依赖操作系统本地窗口系统
- **Swing**：基于 AWT，轻量级，不依赖本地窗口系统

##### 常用 Swing 组件

| 组件 | 说明 |
|:--|:--|
| `JFrame` | 窗口 |
| `JPanel` | 容器面板 |
| `JButton` | 按钮 |
| `JTextField` | 输入框 |
| `JTable` | 表格 |

##### 代码

```java
JFrame jf = new JFrame("登录界面");
JPanel panel = new JPanel();
jf.add(panel);

jf.setSize(400, 300);
jf.setLocationRelativeTo(null); // 居中
jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

JButton jb = new JButton("登录");
jb.setBounds(150, 100, 80, 30);
panel.add(jb);

jf.setVisible(true);
```
