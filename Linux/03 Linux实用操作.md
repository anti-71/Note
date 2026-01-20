# Linux 实用操作

## 第一节 各类小技巧（快捷键）

### 一、ctrl+c 强制停止

- Linux 某些程序的运行，如果想要强制停止它，可以使用快捷键 `ctrl+c`
- 命令输入错误，也可以通过快捷键 `ctrl+c`，退出当前输入，重新输入

### 二、ctrl+d 退出或登出

- 可以通过快捷键：`ctrl+d`，退出账户的登录
- 或者退出某些特定程序的专属页面

**注**	不能用于退出 vi/vim

### 三、历史命令搜素

- 可以通过 history 命令，查看历史输入过的命令
- 可以通过：！命令前缀，自动执行上一次匹配前缀的命令
- 可以通过快捷键：`ctrl+r`，输入内容去匹配历史命令

如果搜索到的内容是你需要的，那么：

- 回车键可以直接执行
- 键盘左右键，可以得到此命令（不执行）

### 四、光标移动快捷键

- `ctrl+a`，跳到命令开头
- `ctrl+e`，跳到命令结尾
- `ctrl +键盘左键`，向左跳一个单词
- `ctrl+键盘右键`，向右跳一个单词

### 五、清屏

- 通过快捷键 `ctrl+l`，可以清空终端内容
- 或通过命令 clear 得到同样效果

## 第二节 软件安装

### 一、Linux 系统的应用商店

操作系统安装软件有许多种方式，一般分为：

- 下载安装包自行安装
  - 如 win 系统使用 exe 文件、msi 文件等
  - 如 mac 系统使用 dmg 文件，pkg 文件等
- 系统的应用商店内安装
  - 如 win 系统有 Microsoft Store 商店
  - 如 mac 系统有 AppStore 商店

Linux 系统同样支持这两种方式

### 二、yum 命令

yum：RPM 包软件管理器，用于自动化安装配置 Linux 软件，并可以自动解决依赖问题

语法：

```bash
yum [-y] [install | remove | search] 软件名称
```

- 选项：-y，自动确认，无需手动确认安装或卸载过程
- install：安装
- remove：卸载
- search：搜索

yum 命令需要 **root 权限**，可以 su 切换到 root, 或使用 sudo 提权。

yum 命令需要 **联网**

### 三、apt 命令-扩展

前面学习的各类 Linux 命令，都是通用的

但是软件安装，CentOS 系统和 Ubuntu 使用不同的包管理器

Centos 使用 yum 管理器，Ubuntu 使用 apt 管理器

语法：

```bash
 apt [-y] [install | remove | search] 软件名称
```

用法和 yum 一致，同样需要 root 权限

- apt install wget，安装 wget
- apt remove wget，移除 wget
- apt search wget，搜索 wget

## 第三节 systemctl

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

## 第四节 软连接

### 一、ln 命令创建软连接

在系统中创建软链接，可以将文件、文件夹链接到其它位置

类似 windows 系统中的《快捷方式》

语法：

```bash
ln -s 参数1 参数2
```

- -s 选项，创建软连接
- 参数 1：被链接的文件或文件夹
- 参数 2：要链接去的目的地

## 第五节 日期和时区

### 一、date 命令

通过 date 命令可以在命令行中查看系统的时间

语法：

```bash
date [-d] [+格式化字符串]
```

- -d 按照给定的字符串显示日期，一般用于日期计算
- 格式化字符串：通过特定的字符串标记，来控制显示的日期格式
  - %Y 年  
  - %y 年份后两位数字(0..99)
  - %m 月份 (01..12)
  - %d 日 (01..31)
  - %H 小时 (0..23)
  - %M 分钟 (00..59)
  - %S 秒 (00..60)
  - %s 自 1970-01-01 00:00:00 UTC 到 现 在 的 秒数

##### 进行日期加减

- -d 选项，可以按照给定的字符串显示日期，一般用于日期计算
- 其中支持的时间标记为：
  - year 年
  - month 月
  - day 天
  - hour 小时
  - minute 分钟
  - second 秒
- -d 选项可以和格式化字符串配合一起使用哦

### 二、修改 Linux 时区

通过 date 查看的日期时间是不准确的，这是因为：系统默认时区非中国的东八区

使用 **root 权限**，执行如下命令，修改时区为东八区时区

```bash
rm -f /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

将系统自带的 localtime 文件删除，并将/usr/share/zoneinfo/Asia/Shanghai 文件链接为 localtime 文件即可

### 三、ntp 程序

我们可以通过 ntp 程序自动校准系统时间

安装 ntp：yum -y install ntp

启动并设置开机自启：

- systemctl start ntpd
- systemctl enable ntpd

当 ntpd 启动后会定期的帮助我们 **联网** 校准系统的时间

也可以手动校准（**需 root 权限**）：ntpdate -u ntp.aliyun.com

通过阿里云提供的服务网址配合 ntpdate（安装 ntp 后会附带这个命令）命令自动校准
