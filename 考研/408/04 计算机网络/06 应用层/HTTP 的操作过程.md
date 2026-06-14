---
名称: HTTP 的操作过程
章: 第六章 应用层
节: "[[第五节 万维网]]"
tags:
  - 知识点
index: 2
---
##### 万维网访问基本流程

从协议执行流程来看，当浏览器要访问某个 WWW 服务器时，首先需完成对该服务器域名的解析

一旦获得其 IP 地址，浏览器便通过 TCP 向该服务器发起连接建立请求

万维网的大致工作过程如下图所示

![image.png|433x339](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/20260612182303779.png)

每个万维网站点都运行一个服务器进程，持续监听 TCP 端口 80（默认）

当监听到连接请求时，服务器便与浏览器建立 TCP 连接

随后，浏览器向服务器发送 HTTP 请求，以获取指定的 Web 页面

服务器收到请求后，组装所请求页面所需的资源，并通过 HTTP 响应返回给浏览器

浏览器对收到的内容进行解析，并将最终的 Web 页面呈现给用户

最后，TCP 连接被释放

##### 访问站点详细步骤（举例）

用户单击鼠标后所发生的事件顺序如下（以访问清华大学网站为例）：

1. 用户在浏览器地址栏中输入 URL：[http://www.tsinghua.edu.cn/index.htm](https://link.wtturl.cn/?target=http%3A%2F%2Fwww.tsinghua.edu.cn%2Findex.htm&scene=im&aid=497858&lang=zh "autolink")
2. 浏览器向 DNS 服务器请求解析 [www.tsinghua.edu.cn](https://link.wtturl.cn/?target=https%3A%2F%2Fwww.tsinghua.edu.cn&scene=im&aid=497858&lang=zh "autolink") 的 IP 地址
3. DNS 系统返回清华大学服务器的 IP 地址
4. 浏览器与该服务器建立 TCP 连接（默认端口号为 80）
5. 浏览器发出 HTTP 请求：GET /index.htm
6. 服务器通过 HTTP 响应把文件 index.htm 发送给浏览器
7. 释放 TCP 连接
8. 浏览器解析 index.htm 文件，并将 Web 页面显示给用户

##### 涉及的 TCP/IP 协议栈

上述过程仅为简化描述

实际上，整个通信可能涉及 TCP/IP 体系结构中的多种协议：

- 应用层的 DHCP、DNS 和 HTTP
- 传输层的 UDP 与 TCP
- 网际层的 IP 和 ARP
- 数据链路层的 CSMA/CD 协议或 PPP（涉及 ISP 接入或广域网传输时）