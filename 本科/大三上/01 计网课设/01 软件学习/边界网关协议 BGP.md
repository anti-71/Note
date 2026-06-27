---
名称: 边界网关协议 BGP
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 13
---
##### 边界网关协议 BGP

##### 构建网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/70fdc4e215a34144b0e977569c650505.png)

路由器×3

##### 配置网络设备

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/3bc4538978224aa58bdfab010942e629.png)

略

##### 配置 BGP 协议

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/0f4eab1fe2c44c8b8623708732240046.png)

单击路由器，进入全局配置模式

`router bgp 200`：开启 BGP 协议，200为自治系统号

`neighbor 10.0.0.1 remote-as 100`：指定邻居，10.0.0.1 为对方端口的 IP 地址，100 为对方的自治系统号

##### 验证连通性

左边路由器`ping`右边路由器，发现无法 ping 通，这是因为还没在 BGP 协议中宣告对方的存在

##### 宣告网段

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/f2f209cde5934dfdaf78eb89ed66476c.png)

`network 10.0.0.0 mask 255.0.0.0`：填写自己网段的网络号和掩码

两个路由器都如此操作后，即可 ping 通
