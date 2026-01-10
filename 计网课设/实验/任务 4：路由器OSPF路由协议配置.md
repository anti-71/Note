# 任务 4：路由器OSPF路由协议配置

## 一、目的和要求

**设计目的**：

掌握在同一网络中同时运行多种路由协议的配置方法，学习**路由重发布（Redistribution）**技术实现不同协议间的互通。

**任务背景**：

假设某公司通过一台三层交换机连到公司出口路由器R1上，路由器在和公司外的另一台路由器R2连接，三层交换机与R1间运行RIPV2路由协议，R1与R2间运行OSPF路由协议。现在要做适当的配置，实现公司内部与公司外部主机之间的相互通信。

## 二、实验环境

**软件环境**：

Cisco Packet Tracer 8.2。

**硬件设备**：

PC 2台、三层交换机（S3560）1台、Router-PT 2台、DCE串口线、直通线、交叉线。

**IP配置内容**：

- **PC配置**：PC1(192.168.1.2/24,网关192.168.1.1)；PC2(192.168.2.2/24,网关192.168.2.1) 。
- **S3560配置**：VLAN10(192.168.1.1/24)连接内网主机；VLAN20(192.168.3.1/24)连接R1 。
- **R1配置**：Fa0/0(192.168.3.2/24)连接S3560；Serial2/0(192.168.4.1/24)连接R2 。
- **R2配置**：Serial2/0(192.168.4.2/24)连接R1；Fa0/0(192.168.2.1/24)连接PC2 。

## 三、实验内容

### 第一步：基础协议配置

**S3560（运行RIPV2）**：

```
S3560(config)#router rip
S3560(config-router)#version 2
S3560(config-router)#network 192.168.1.0
S3560(config-router)#network 192.168.3.0
```

**R2（运行OSPF）**：

```
R2(config)#router ospf 100
R2(config-router)#network 192.168.2.0 0.0.0.255 area 0
R2(config-router)#network 192.168.4.0 0.0.0.255 area 0
```

### 第二步：R1上的路由重分布

R1需配置双协议并互相引入：

```
R1(config)#router rip
R1(config-router)#version 2
R1(config-router)#network 192.168.3.0
R1(config-router)#redistribute ospf 100 metric 10   #将OSPF引入RIP
R1(config)#router ospf 100
R1(config-router)#network 192.168.4.0 0.0.0.255 area 0
R1(config-router)#redistribute rip subnets        #将RIP引入OSPF
```

## 四、实验结果

- 使用`show ip route`查看R1和R2的路由表，确认是否学习到对方协议的路由。

<img src="C:\Users\Syz13\AppData\Roaming\Typora\typora-user-images\image-20260110154148484.png" alt="image-20260110154148484" style="zoom:50%;" />

<img src="C:\Users\Syz13\AppData\Roaming\Typora\typora-user-images\image-20260110154201986.png" alt="image-20260110154201986" style="zoom:50%;" />

<img src="C:\Users\Syz13\AppData\Roaming\Typora\typora-user-images\image-20260110154216414.png" alt="image-20260110154216414" style="zoom:50%;" />

- 在PC1上`ping 192.168.2.2`，若通畅则表示配置成功。

<img src="C:\Users\Syz13\AppData\Roaming\Typora\typora-user-images\image-20260110154124323.png" alt="image-20260110154124323" style="zoom:50%;" />

## 五、心得体会

通过本次路由综合配置实验，我深入理解了多协议混合网络环境下路由重分布（Redistribution）的必要性。

实验中，我掌握了在R1上同时运行RIPV2与OSPF，并通过重分布命令将不同协议的度量值进行转换。我意识到，不同协议对链路成本的计算方式不同，必须通过精确的参数设置（如Metric和Subnets）才能确保数据包在内外网间正确路由。这次实验提升了我在复杂拓扑中解决协议不兼容问题的能力，让我对大型企业网中不同区域采用差异化管理策略的实际场景有了深刻体会，增强了网络集成的实战经验。