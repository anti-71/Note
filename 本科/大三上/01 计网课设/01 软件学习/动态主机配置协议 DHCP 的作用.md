---
名称: 动态主机配置协议 DHCP 的作用
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 18
---
##### 动态主机配置协议 DHCP 的作用

##### 构建网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/195f855728074210bad923fc25386898.png)

计算机×2 + 交换机 + 路由器 + 服务器×3

##### 配置网络设备

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/b4954962aba94318976624bed55ab091.png)

略（勿忘配置 Web 服务器的默认路由和打开 DNS 服务器的 DNS 服务）

##### 配置 DHCP 服务器

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/2b61bae65ce6436e9271d6981f87b177.png)

单击 DHCP 服务器，选择“服务” - “DHCP”，将服务设为开，设置“默认网关”和“DNS 服务器”，最后点击“保存”

将两个主句的 IP 配置改为 DHCP

##### 跟踪数据包

打开任一主机的“网页浏览器”，输入域名，显示内容即实验成功
