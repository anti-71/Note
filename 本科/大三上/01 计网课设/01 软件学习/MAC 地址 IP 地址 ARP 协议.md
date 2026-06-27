---
名称: MAC 地址 IP 地址 ARP 协议
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 2
---
##### MAC 地址 IP 地址 ARP 协议

##### 构建网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/8ffae367e44c44ed8bfb113372d98534.png)

计算机 + 计算机

##### 配置网络设备

##### 配置计算机 IP 地址

计算机 1：IPv4 Address 设置为192.168.0.1

计算机 2：IPv4 Address 设置为192.168.0.2

##### 查看计算机的 ARP 表或端口状态汇总表

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/8b2662708b9e4e81b64a593e376dc7a8.png)

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/9eecab78ffe845868060c359748fcdec.png)

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/2947e60de2414d17a5b06d4043de0d86.png)

##### 跟踪数据包

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/a1cb1e0ee0dc47db868e3249fc642eeb.png)

点击“添加简单的 PDU”，依次点击计算机 1、计算机 2

##### 数据包发送流程

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/5effc93f974642459589c534f7605037.png)

##### 阶段 1：初始化

1. 计算机 1 先发起通信准备
   - 0.000 秒：计算机 1 同时发送 ARP（获取对方 MAC 地址）和 ICMP（尝试连通）
2. 计算机 2 响应
   - 0.001 秒：计算机 2 收到请求，回复 ARP（告知自己的 MAC 地址）
   - 0.002 秒：计算机 1 收到计算机 2 的 ARP 响应，完成 “地址解析”

##### 阶段 2：持续通信

1. ICMP 连通性测试循环
   - 0.003 秒：计算机 1 发 ICMP 给计算机 2
   - 0.004 秒：计算机 2 回复 ICMP 给计算机 1
2. 再次尝试发送 ICMP（直接发即可）
   - 582.141 秒：计算机 1 再次发 ICMP 给计算机 2
   - 582.142 秒：计算机 2 回复 ICMP

##### 清除 ARP 表

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/84f867783d3b403ab591ed00594bb452.png)

打开计算机的“命令提示符”

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/ec5569fee69e4081a0749896c90b2125.png)

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/31921b1608ce412bb8e5c9e5a6ce65d1.png)

`arp -a`：显示当前计算机的 ARP 表

`arp -d`：清除当前计算机的 ARP 表
