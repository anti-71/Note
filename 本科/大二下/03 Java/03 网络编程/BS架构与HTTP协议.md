---
名称: BS 架构与 HTTP 协议
章: 03 Java
节: "[[第三章 网络编程]]"
tags:
  - 知识点
index: 7
---
##### B/S 架构原理

服务端接收浏览器连接，响应 HTTP 协议格式的网页数据

> [!note]
> 服务器必须给浏览器响应 HTTP 协议规定的数据格式，否则浏览器不识别返回的数据

##### HTTP 响应格式

```
HTTP/1.1 200 OK
Content-Type:text/html;charset=utf-8

<html>
<body>
<h1>网页内容</h1>
</body>
</html>
```

##### 代码：响应网页

```java
ServerSocket ss = new ServerSocket(8080);
while (true) {
    Socket socket = ss.accept();
    new ServerReader(socket).start();
}
```

```java
class ServerReader extends Thread {
    private Socket socket;
    public ServerReader(Socket socket) { this.socket = socket; }

    @Override
    public void run() {
        PrintStream ps = new PrintStream(socket.getOutputStream());
        ps.println("HTTP/1.1 200 OK");
        ps.println("Content-Type:text/html;charset=utf-8");
        ps.println();
        ps.println("<html><body><h1>Hello</h1></body></html>");
        ps.close();
        socket.close();
    }
}
```

##### 线程池优化

高并发时，应为每个连接使用线程池而非新线程，避免资源耗尽
