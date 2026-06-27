---
名称: TCP 多发多收与多客户端
章: 03 Java
节: "[[第三章 网络编程]]"
tags:
  - 知识点
index: 6
---
##### 多发多收

客户端在循环中不断发送消息，服务端在循环中不断接收

##### 多客户端支持

服务端主线程负责接收连接，每收到一个 Socket 就创建子线程处理

```java
// 服务端主线程
ServerSocket ss = new ServerSocket(9999);
while (true) {
    Socket socket = ss.accept();
    new ServerReader(socket).start();
}

// 子线程
class ServerReader extends Thread {
    private Socket socket;
    public ServerReader(Socket socket) { this.socket = socket; }

    @Override
    public void run() {
        DataInputStream dis = new DataInputStream(socket.getInputStream());
        while (true) {
            String msg = dis.readUTF();
            System.out.println("收到：" + msg);
        }
    }
}
```
