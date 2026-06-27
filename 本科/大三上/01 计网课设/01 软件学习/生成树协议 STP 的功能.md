---
名称: 生成树协议 STP 的功能
章: 01 计网课设
节: "[[第一章 软件学习]]"
tags: [知识点]
index: 6
---
##### 生成树协议 STP 的功能

##### 构建网络拓扑

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/9f1f94c2666449c39a574fb0ef6f5651.png)

计算机×2 + 交换机×4（环路）

有一个灯为橙色的原因是系统拒绝环路（由 STP 协议控制）

##### 配置网络设备

计算机 1：IPv4 Address 设置为192.168.0.1

计算机 2：IPv4 Address 设置为192.168.0.2

##### 验证主机的连通性

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/4e95c289f4df4198b62749c21f180188.png)

单击“计算机”，打开“命令提示符”，输入`ping IP地址`，若收到四条响应，则认为两条计算机连通

##### 断开端口

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/3e072eaf74724862a0503751af63c068.png)

单击“交换机”，选择“配置”，找到通路标签对应的接口项，切换“端口状态”

此时再`ping`主机，显然不连通，但是之前的橙灯变绿，又已连通

##### 关闭 STP 协议

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/ac9a79daf60c49fc86c9598d252d3004.png)

进入“交换机”的“命令行界面”，输入`en`进入特权模式，输入`config`进入**配置模式**，按下回车

`no spanning-tree vlan 1`：关闭 STP 协议

四个交换机都如上操作

此时打开之前关闭的端口，刷新几次，发现所有灯都绿了

##### 跟踪数据包

![](https://shiyuzhe.oss-cn-hangzhou.aliyuncs.com/Tuchuang/421b32dba52c43968de19f9ef9f3aed3.png)

广播一个数据包，数据如上设置

运行发现，环路中一直在兜圈子

`ping`主机，发现超时，无法连通
