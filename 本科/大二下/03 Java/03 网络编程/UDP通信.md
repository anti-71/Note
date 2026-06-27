---
名称: UDP 通信
章: 03 Java
节: "[[第三章 网络编程]]"
tags:
  - 知识点
index: 4
---
##### DatagramSocket 与 DatagramPacket

Java 通过 `DatagramSocket`（发送/接收端）和 `DatagramPacket`（数据包）实现 UDP 通信

##### 客户端（发送端）

```java
// 一发
DatagramSocket socket = new DatagramSocket();
byte[] bytes = "你好".getBytes();
DatagramPacket packet = new DatagramPacket(bytes, bytes.length,
        InetAddress.getLocalHost(), 8080);
socket.send(packet);

// 多发多收
Scanner sc = new Scanner(System.in);
while (true) {
    String msg = sc.nextLine();
    if ("exit".equals(msg)) break;
    DatagramPacket p = new DatagramPacket(msg.getBytes(),
            msg.getBytes().length, InetAddress.getLocalHost(), 8080);
    socket.send(p);
}
```

##### 服务端（接收端）

```java
DatagramSocket socket = new DatagramSocket(8080);
byte[] buf = new byte[1024 * 64];
DatagramPacket packet = new DatagramPacket(buf, buf.length);

while (true) {
    socket.receive(packet);
    int len = packet.getLength();
    String data = new String(buf, 0, len);
    System.out.println("收到：" + data);
}
```
