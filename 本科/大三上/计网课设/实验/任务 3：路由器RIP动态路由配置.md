# 任务 3：路由器RIP动态路由配置

## 一、目的和要求

**设计目的**：

掌握RIP协议的配置方法，学习如何通过动态路由协议自动生成路由表，并熟悉广域网线缆的连接方式。

**实验背景**：

假设校园网通过一台三层交换机连到校园网出口路由器上，路由器再和校园外的另一台路由器连接。现在要做适当配置，实现校园网内部主机与校园外部主机之间的相互通信。为了简化网管的管理维护工作，分别采用RIPV2和OSPF协议实现互通。

## 二、实验环境

**软件环境**：

Cisco Packet Tracer 8.2。

**硬件设备**：

PC 2台、Switch_3560 1台、Router-PT 2台、DCE串口线、交叉线、直连线。

**IP地址规划** ：

| **设备** | **接口/VLAN**     | **IP地址**  | **掩码**      |
| -------- | ----------------- | ----------- | ------------- |
| PC1      | 网卡              | 192.168.1.2 | 255.255.255.0 |
| S3560    | VLAN 10 (网关)    | 192.168.1.1 | 255.255.255.0 |
| S3560    | VLAN 20 (连R1)    | 192.168.3.1 | 255.255.255.0 |
| R1       | Fa 0/0 (连S3560)  | 192.168.3.2 | 255.255.255.0 |
| R1       | Serial 2/0 (连R2) | 192.168.4.1 | 255.255.255.0 |
| R2       | Serial 2/0 (连R1) | 192.168.4.2 | 255.255.255.0 |
| R2       | Fa 0/0 (连PC2)    | 192.168.2.1 | 255.255.255.0 |
| PC2      | 网卡              | 192.168.2.2 | 255.255.255.0 |

## 三、实验内容

### RIPV2协议配置步骤

**S3560配置：**

```
S3560(config)#router rip
S3560(config-router)#version 2
S3560(config-router)#network 192.168.1.0
S3560(config-router)#network 192.168.3.0
```

**R1配置：**

```
R1(config)#router rip
R1(config-router)#version 2
R1(config-router)#network 192.168.3.0
R1(config-router)#network 192.168.4.0
```

**R2配置：**

```
R2(config)#router rip
R2(config-router)#version 2
R2(config-router)#network 192.168.2.0
R2(config-router)#network 192.168.4.0
```

### OSPF协议配置步骤

在同一实验中切换协议时，建议先删除旧协议进程以防干扰。

#### 第一步：删除RIP进程

```
(config)#no router rip
```

#### 第二步：OSPF协议配置  

**S3560配置：**

```
S3560(config)#router ospf 100
S3560(config-router)#network 192.168.1.0 0.0.0.255 area 0
S3560(config-router)#network 192.168.3.0 0.0.0.255 area 0
```

**R1配置：**

```
R1(config)#router ospf 100
R1(config-router)#network 192.168.3.0 0.0.0.255 area 0
R1(config-router)#network 192.168.4.0 0.0.0.255 area 0
```

**R2配置：**

```
R2(config)#router ospf 100
R2(config-router)#network 192.168.2.0 0.0.0.255 area 0
R2(config-router)#network 192.168.4.0 0.0.0.255 area 0
```

## 四、实验结果

**查看路由表**：

执行`show ip route`。RIP学习到的路由标记为**R**，OSPF学习到的标记为**O**。

**连通性**：

PC1与PC2互相Ping通即为成功

## 五、心得体会

通过本次动态路由配置实验，我对比学习了RIPV2与OSPF协议的配置逻辑。实验中，我掌握了在三层交换机开启`ip routing`的关键操作，并理解了动态路由协议如何自动维护路由表以实现校园网内外通信。相比静态路由，动态路由在复杂拓扑中更具灵活性。在协议切换过程中，我认识到宣告网段及反掩码（OSPF）准确性的重要。这次实验不仅提升了我的多设备联动调试能力，更让我深刻体会到不同协议在收敛速度与规模适应性上的差异，为构建高效自动化网络积累了实践经验。