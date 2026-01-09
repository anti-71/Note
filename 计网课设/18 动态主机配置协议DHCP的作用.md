# 动态主机配置协议$DHCP$的作用

## 构建网络拓扑

<img src="D:\Note\计网课设\Assets\18.1.png" style="zoom:50%;" />

计算机×2 + 交换机 + 路由器 + 服务器×3

## 配置网络设备

<img src="D:\Note\计网课设\Assets\18.2.png" style="zoom:50%;" />

略（勿忘配置$Web$服务器的默认路由和打开$DNS$服务器的$DNS$服务）

##### 配置$DHCP$服务器

<img src="D:\Note\计网课设\Assets\18.3.png" style="zoom:50%;" />

单击$DHCP$服务器，选择“服务” - “$DHCP$”，将服务设为开，设置“默认网关”和“$DNS$服务器”，最后点击“保存”

将两个主句的$IP$配置改为$DHCP$

## 跟踪数据包

打开任一主机的“网页浏览器”，输入域名，显示内容即实验成功