---
名称: VLAN 间通信的实现方法：多臂路由
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 15
---
##### VLAN 间通信的实现方法：多臂路由

##### 构建网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/35423d749cfe4175bdc2ac02a25e0511.png)

计算机×6 + 交换机 + 路由器

##### 配置网络设备

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/1afe6124684b4d2fb1b6d5030b546c0f.png)

略（勿忘配置默认路由和 VLAN，交换机和路由器端口）

##### 跟踪数据包

`ping`、发送简单 PDU，确认两个 VLAN 间是否能够通信

实际应用中不常用
