---
名称: VLAN 间通信的实现方法：单臂路由
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 16
---
##### VLAN 间通信的实现方法：单臂路由

##### 构建网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/9e59ac00c12b49ab9ed0fc011c3034e0.png)

计算机×6 + 交换机 + 路由器

##### 配置网络设备

如上一部分的图，略（勿忘配置默认路由和 VLAN）

##### 配置逻辑子接口

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/912830735a1b403d830ed49d4b86d134.png)

打开路由器命令行，进入特权模式，终端配置模式

`interface g0/0.1`：将接口“g0/0”分出一个逻辑子接口

`encapsulation dot1q 10`：在路由器子接口上启用 IEEE 802.1Q 协议，10 为 VLAN 号

`no shutdown`：启用接口

##### 更改路由器和交换机接口模式

将接口模式改为“Trunk”

##### 跟踪数据包

`ping`、发送简单 PDU，确认两个 VLAN 间是否能够通信

随着 VLAN 间通信流量的增大，路由器可能成为网络的瓶颈
