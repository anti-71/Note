---
名称: IP 地址与 InetAddress
章: 03 Java
节: "[[第三章 网络编程]]"
tags:
  - 知识点
index: 2
---
##### IP 地址类型

- **IPv4**：32 位，点分十进制
- **IPv6**：128 位，冒分十六进制

##### 域名与 DNS

域名是用于识别和定位网站的人类可读名称，DNS 将域名解析为 IP 地址

##### 公网 IP 与内网 IP

- 公网 IP：可连接互联网
- 内网 IP：局域网使用，如 `192.168.x.x`

##### 本机 IP

`127.0.0.1`、`localhost`：代表本机

##### 常用命令

- `ipconfig`：查看本机 IP
- `ping IP`：检查网络是否连通

##### InetAddress 常用方法

| 方法 | 说明 |
|:--|:--|
| `getLocalHost()` | 获取本机 IP 对象 |
| `getByName(String host)` | 根据 IP 或域名获取对象 |
| `getHostName()` | 获取主机名 |
| `getHostAddress()` | 获取 IP 地址 |
| `isReachable(int timeout)` | 判断主机是否连通 |

```java
InetAddress ip1 = InetAddress.getLocalHost();
System.out.println(ip1.getHostName()); // 主机名
System.out.println(ip1.getHostAddress()); // IP

InetAddress ip2 = InetAddress.getByName("www.baidu.com");
System.out.println(ip2.isReachable(5000)); // 是否互通
```
