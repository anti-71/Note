---
名称: 开放路径最短协议 OSPF
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 12
---
##### 开放路径最短协议 OSPF

##### 构建网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/30eb07bf10e34b25a102cc5891b8c111.png)

计算机×2 + 路由器（2911）×3

左下路由器与上方路由器采用“串行 DTE”相连

##### 配置网络设备

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/aadcd50f610b4292aab591dd8fab02ca.png)

略（勿忘配置默认路由）

##### 配置 OSPF 协议

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/93d1c57c88594e4cb7cdbf79185e0a98.png)

单机路由器打开命令行，进入特权模式的配置模式：

`router ospf 100`：开启该路由器的 OSPF 协议，进程号随意

`network 30.0.0.0 0.255.255.255 area 0`：宣告网段，网络号 + 掩码的反码 + 区域号

配置好三个路由器，此时两个主机可互相 ping 通

##### 跟踪数据包

两台主机间发送一个简单的 PDU，通过右下的路由器，验证了 OSPF 协议
