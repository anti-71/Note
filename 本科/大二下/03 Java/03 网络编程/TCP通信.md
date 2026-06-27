---
名称: TCP 通信
章: 03 Java
节: "[[第三章 网络编程]]"
tags:
  - 知识点
index: 5
---
##### Socket 与 ServerSocket

TCP 通信通过 `Socket`（客户端）和 `ServerSocket`（服务端）实现

##### 客户端一发一收

```java
Socket socket = new Socket("127.0.0.1", 9999);
OutputStream os = socket.getOutputStream();
DataOutputStream dos = new DataOutputStream(os);
dos.writeInt(1);
dos.writeUTF("你好");
socket.close();
```

##### 服务端一发一收

```java
ServerSocket ss = new ServerSocket(9999);
Socket socket = ss.accept(); // 阻塞等待连接
InputStream is = socket.getInputStream();
DataInputStream dis = new DataInputStream(is);
int id = dis.readInt();
String msg = dis.readUTF();
System.out.println("收到：" + msg);
```
