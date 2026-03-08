# $VLAN$间通信的实现方法：多臂路由

## 构建网络拓扑

<img src="D:\Note\计网课设\Assets\15.1.png" style="zoom:50%;" />

计算机×6 + 交换机 + 路由器

## 配置网络设备

<img src="D:\Note\计网课设\Assets\15.2.png" style="zoom:50%;" />

略（勿忘配置默认路由和$VLAN$，交换机和路由器端口）

## 跟踪数据包

`ping`、发送简单$PDU$，确认两个$VLAN$间是否能够通信

实际应用中不常用