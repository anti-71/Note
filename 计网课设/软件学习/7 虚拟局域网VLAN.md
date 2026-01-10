# 虚拟局域网$VLAN$

## 构建网络拓扑

<img src="D:\Note\计网课设\Assets\7.1.png" style="zoom:50%;" />

计算机×6 + 交换机（星形结构）

## 配置网络设备

略

##### 查看端口号改进

<img src="D:\Note\计网课设\Assets\7.2.png" style="zoom:50%;" />

“选项” - ”首选项“ ，勾选“当鼠标在逻辑工作区中的设备上悬停时显示端口标签”

也可以自己设置标签

## 划分$VLAN$

##### 验证广播域

任意选择一个计算机发送广播$PDU$，发现其他主机都能收到，为一个广播域

##### 创建一个新的$VLAN$

现在我们希望左边 3 台主机同属于一个$VLAN$

<img src="D:\Note\计网课设\Assets\7.3.png" style="zoom:50%;" />

单击“交换机”，选择“配置” - “$VLAN$数据库”

按上图填写信息，点击“添加”，即在交换机内创建了一个新的"$VLAN2$"

##### 设置$VLAN2$对应的接口

<img src="D:\Note\计网课设\Assets\7.4.png" style="zoom:50%;" />

确定连接主机的端口号，打开交换机对应的接口，将“$VLAN$”切换为对应的$VLAN$号

另外两台主机同理

此时再发送一个广播$PDU$，发现只能发送到左边两台主机上

##### 创建一个新的$VLAN$（命令行）

现在我们以命令行的形式将右边三台主机配置到$VLAN3$

<img src="D:\Note\计网课设\Assets\7.5.png" style="zoom:50%;" />

此时命令行界面如上，先输入`end`退出对端口的配置

再输入`exit`退出特权模式同时刷新界面

<img src="D:\Note\计网课设\Assets\7.6.png" style="zoom:50%;" />

再次进入特权模式

`config terminal`：终端的配置模式

`vlan 3`：创建一个$VLAN$号为 3 的$VLAN$

`name VLAN3`：将该$VLAN$命名为$VLAN3$

此时已经创建好一个$VLAN3$了

<img src="D:\Note\计网课设\Assets\7.7.png" style="zoom:50%;" />

`show vlan brief`：展示这个交换机的所有$VLAN$的简洁信息

##### 设置$VLAN3$对应的接口（命令行）

<img src="D:\Note\计网课设\Assets\7.8.png" style="zoom:50%;" />

重新进入终端的配置模式

`interface range fastethernet 0/4-6`：将端口0/4、0/5、0/6一起配置

`switch mode access`：将端口设为接入模式

`switch access vlan 3`：将端口接入$VLAN3$

此时再发送一个广播$PDU$，发现只能发送到右边两台主机上

## 更改网络拓扑

<img src="D:\Note\计网课设\Assets\7.9.png" style="zoom:50%;" />

增加计算机×6 + 交换机

## 配置新增网络设备

$IP$地址设置略

左下角三个主机分配到$VLAN2$

右下角三个主机分配到$VLAN3$

## 跟踪数据包

从源$IP$地址 192.168.0.1 发送一个广播$PDU$，发现仍只能发送给另外两台主机，而不能传到左下角

是因为连接两个交换机的端口处于$VLAN1$（默认），$VLAN2$的数据包无法从该端口发出

##### 更改连接端口模式

<img src="D:\Note\计网课设\Assets\7.10.png" style="zoom:50%;" />

将两个交换机连接的端口从“$Access$”模式切换到“$Trunk$”模式，该模式能传输所有$VLAN$

此时再发送广播$PDU$，已经能够发送到左下角三台主机了