---
名称: IPv4 地址：构造超网（无分类聚合）
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 9
---
##### IPv4 地址：构造超网（无分类聚合）

##### 构造网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/c085d511442b4e5abf89198d208f58d8.png)

计算机×5 + 交换机×2 + 路由器（2911，多端口需求）×2

##### 配置网络设备

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/6fd1fe6b9c084f23a530100baf95874f.png)

按照上图先配置 IPv4 Address、子网掩码、默认网关

##### 验证网络连通性

左边 4 个主机互相都能 ping 通，但 ping 右边的主机显示不可达

原因是左边那个路由器不知道右边的网络，需要我们设置静态路由

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/fba7c74841a8463fad661cb28c8f5530.png)

单击左边的路由器，选择“配置” - “静态”，按上图填写，其中“网络”填写目的网络的网络号

右边的路由器同理

此时再`ping`，已经可以 ping 通

##### 路由聚合

如上所学，对右边的路由器填写静态路由要对上下两个网络都填写，此时可以路由聚合，即将这两条并为一条：

删除这两条静态路由，添加：

​	网络：192.168.16.0

​	掩码：255.255.255.0

​	下一跳：192.168.16.193

这样，也能 ping 通
