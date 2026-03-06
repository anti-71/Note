---
名称: systemctl
章: 01 Linux
节: "[[第三章 Linux 实用操作]]"
tags:
  - 知识点
index: 3
---
### 一、systemctt 命令

Linux 系统很多软件（内置或第三方）均支持使用 systemctl 命令控制：启动、停止、开机自启

能够被 systemctl 管理的软件，一般也称之为：服务

语法:

```bash
systemctl start | stop | status | enable | disable 服务名
```

- start 启动
- stop 关闭
- status 查看状态
- enable 开启开机自启
- disable 关闭开机自启

系统内置的服务比较多，比如：

- NetworkManager，主网络服务
- network，副网络服务
- firewalld，防火墙服务
- sshd，ssh 服务（FinalShell 远程登录 Linux 使用的就是这个服务）

除了内置的服务以外，部分第三方软件安装后也可以以 systemctl 进行控制

- yum install -y ntp，安装 ntp 软件
  - 可以通过 ntpd 服务名，配合 systemctl 进行控制
- yum install -y httpd，安装 apache 服务器软件
  - 可以通过 httpd 服务名，配合 systemctl 进行控制