---
名称: IPv4 地址：分类地址
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 8
---
##### IPv4 地址：分类地址

##### 构建网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/5e43c3a195994ff8894f8e8ba7e44b89.png)

计算机×2 + 路由器

##### 配置网络设备

计算机 1：IPv4 Address 设置为192.168.0.1

计算机 2：IPv4 Address 设置为172.16.0.1

##### 配置路由器端口

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/36d5c0f9945e46dd8c5456945b2a86d5.png)

单击“路由器”，选择“配置” - 对应接口

勾选“端口状态”为开，IPv4 Address 设置为 192.168.0.254

以端口 0 为例，要与主机的网络号相同，最后一位一般往大取（不取 255），端口 2 同理

##### 验证网络连通性

此时由计算机 0 `ping`另一个计算机，发现无法 ping 通

这是主机还没设置默认路由，只要将默认路由改成对应 IPv4 Address 即可

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/151ea9312e5144b9b32e916a7dc140ff.png)

此时再`ping`，第一个会超时，原因是主机不知道对方的 MAC 地址，先发送 ARP 广播请求，系统视为超时
