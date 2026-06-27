---
名称: 路由信息协议 RIP
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 11
---
##### 路由信息协议 RIP

##### 构建网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/30eb07bf10e34b25a102cc5891b8c111.png)

计算机×2 + 路由器（2911）×3

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/81b457c4a3834b5480a5e5955a6c9f2d.png)

左下路由器与上方路由器采用“串行 DTE”相连

##### 串行 DTE 的连接

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/ea73fbcd64304d7288dd90044c49878a.png)

单击左下的路由器，先关闭电源，选择“HWIC-2T”拖入图示插槽位置，开启电源

两个路由器都安装好后，可连接“串行 DTE”，端口二选一即可（但要相对应）

##### 配置网络设备

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/aadcd50f610b4292aab591dd8fab02ca.png)

略（勿忘配置默认路由）

##### 配置 RIP 协议

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/dd1c6ea9dd2b4519856965902dc2571f.png)

单击路由器，选择“配置” - “RIP”，填写每个端口所在网络号，点击“添加”

三个路由器都如此配置，此时两台主机可互相 ping 通

##### 跟踪数据包

两台主机间发送一个简单的 PDU，不通过右下的路由器，验证了 RIP 协议

右下的路由器`ping 30.0.0.1`，走哪个路由器都是等效的，实际上 ping 四次，两条路径交替走（负载平衡）
