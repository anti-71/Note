# 边界网关协议$BGP$

## 构建网络拓扑

<img src="D:\Note\计网课设\Assets\13.1.png" style="zoom:50%;" />

路由器×3

## 配置网络设备

<img src="D:\Note\计网课设\Assets\13.2.png" style="zoom:50%;" />

略

##### 配置$BGP$协议

<img src="D:\Note\计网课设\Assets\13.3.png" style="zoom:50%;" />

单击路由器，进入全局配置模式

`router bgp 200`：开启$BGP$协议，200为自治系统号

`neighbor 10.0.0.1 remote-as 100`：指定邻居，10.0.0.1 为对方端口的$IP$地址，100 为对方的自治系统号

## 验证连通性

左边路由器`ping`右边路由器，发现无法$ping$通，这是因为还没在$BGP$协议中宣告对方的存在

##### 宣告网段

<img src="D:\Note\计网课设\Assets\13.4.png" style="zoom:50%;" />

`network 10.0.0.0 mask 255.0.0.0`：填写自己网段的网络号和掩码

两个路由器都如此操作后，即可$ping$通

