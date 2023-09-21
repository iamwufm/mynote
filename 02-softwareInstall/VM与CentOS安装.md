
## 一、软件包的下载
### 1.1 centos下载
CentOS的官网下载路径：[Download (centos.org)](https://www.centos.org/download/)
官网的版本最新到7.9，提供到2024年，后面会转为centOS Stream(实时更新，非稳定版本)。

![[Pasted image 20230818134859.png]]

![[Pasted image 20230818134953.png]]

![[Pasted image 20230818135051.png]]

### 1.2 VM下载

一台电脑本身可以安装几个操作系统，但是做不到多个系统来回切换。
我们需要一个软件，可以虚拟出几个电脑，然后在上面安装操作系统，甚至可以在一台计算机上将几个虚拟机系统连接为一个局域网或者连接到互联网中。VMware这是这么一个软件。

VMware官网下载路径：[VMware - Delivering a Digital Foundation For Businesses](https://www.vmware.com/)

目前最新版本是17。
这个软件是需要购买的，可以在网上查找解决方案，如查找许可证。
![[Pasted image 20230818140951.png]]

### 1.3 SecureCRT下载
SecureCRT官网下载路径：[VanDyke Software: Secure Shell Solutions - Secure File Transfer, Secure Terminal Emulation, SSH, SSH2, SFTP, FTP](https://www.vandyke.com/)


![[Pasted image 20230818172113.png]]

![[Pasted image 20230818172214.png]]

![[Pasted image 20230818172245.png]]

### 1.4 Xshell和Xftp下载
Xshell和Xftp官网下载路径：[家庭/学校免费 - NetSarang Website (xshell.com)](https://www.xshell.com/zh/free-for-home-school/)

## 二、VM的安装

双击应用程序安装，安装后输入许可证，即可使用VM软件了。

安装VM后，会有VM1和VM8两块网卡，VM8是虚拟机使用NAT模式上网的网卡。
![[Pasted image 20230818145802.png]]

## 三、CentOS的安装

系统的安装分为两个步骤，第一步是配置一台电脑，选配cpu、内存、磁盘、网卡等硬件。第二部才是安装系统。
### 3.1 配置电脑
双击打开VM软件。

![[Pasted image 20230818143041.png]]

![[Pasted image 20230818143129.png]]

![[Pasted image 20230818143219.png]]

![[Pasted image 20230818143300.png]]

![[Pasted image 20230818143405.png]]

![[Pasted image 20230818143639.png]]

选择CPU的个数，有个原则就是选满（跟物理机的CPU个数相同，但是不能超过）。

查看物理机CPU的个数


![[Pasted image 20230818144307.png]]

**建议给4g，不能给太多，后期多个虚拟机一起启动。**

![[Pasted image 20230818144805.png]]

![[Pasted image 20230818144909.png]]

![[Pasted image 20230818144929.png]]

![[Pasted image 20230818145038.png]]

![[Pasted image 20230818145018.png]]

![[Pasted image 20230818145132.png]]

![[Pasted image 20230818145405.png]]

![[Pasted image 20230818145459.png]]
### 3.2 安装centOS系统

在安装系统前需要检查自己bios的虚拟化设置是否打开（大部分电脑默认是打开的，可以尝试直接安装，如果出错再去调试也行）。
![[Pasted image 20230818150330.png]]

![[Pasted image 20230818150627.png]]

![[Pasted image 20230818150810.png]]


![[Pasted image 20230818150912.png]]


![[Pasted image 20230818151121.png]]

![[Pasted image 20230818151326.png]]

![[Pasted image 20230818151901.png]]

![[Pasted image 20230818151941.png]]

等待安装。

![[Pasted image 20230818152639.png]]

密码：abcd1234..

![[Pasted image 20230818152614.png]]

把ip地址修改为静态ip。ip和网关参考[[VM与CentOS安装#3.2.1 可用ip范围]]
 ```shell
# 1.修改网卡配置：
vi /etc/sysconfig/network-scripts/ifcfg-ens33

# 修改内容如下：
BOOTPROTO=static # dhcp动态ip地址，static静态ip地址
ONBOOT=yes # yes开启网卡自动连接
IPADDR=修改没有冲突的ip
# 子网掩码
NETMASK=255.255.255.0
# 网关
GATEWAY=192.168.204.2
# DNS是用来做域名解析的
DNS1=192.168.204.2

# 2.修改完成后，重启网络服务
systemctl restart network
```

#### 3.2.1 可用ip范围

查看虚拟机vm8的网关和已经被占用的ip。

![[Pasted image 20230818154610.png]]


![[Pasted image 20230818154819.png]]

![[Pasted image 20230818164646.png]]

![[Pasted image 20230830093458.png]]

ip:192.168.204.1也不可用，综合上一张图片。**可用ip范围是192.168.203.3~254**
## 四、克隆机器

机器在关机的状态下进行克隆。

![[Pasted image 20230818165052.png]]

![[Pasted image 20230818165143.png]]

![[Pasted image 20230818165208.png]]

![[Pasted image 20230818165229.png]]

![[Pasted image 20230818165321.png]]

开机后进行如下操作：

```shell
# 1.修改网卡配置：
vi /etc/sysconfig/network-scripts/ifcfg-ens33
# 修改内容如下：
BOOTPROTO=static # dhcp动态ip地址，static静态ip地址
# UUID那行删除，开机会自动分配，避免与克隆那台机器一样
ONBOOT=yes # yes开启网卡自动连接
IPADDR=修改没有冲突的ip
# 子网掩码
NETMASK=255.255.255.0
# 网关
GATEWAY=192.168.204.2
# DNS是用来做域名解析的
DNS1=192.168.204.2

# 2.修改主机名
vi /etc/hostname

# 3.重启生效
reboot
```

## 五、远程终端工具安装

### 5.1 安装远程登陆工具
在真实服务器操作不方便，我们需要远程登陆来操作。比较主流的远程登陆工具有：Xshell，SecureCRT等。
Xshell有免费版，SecureCRT是需要收费的。

#### 5.1.1 安装SecureCRT
双击《scrt-x64-bsafe.9.4.1.3102.exe》应用程序
![[Pasted image 20230818173013.png]]

![[Pasted image 20230818173036.png]]

![[Pasted image 20230818173112.png]]

![[Pasted image 20230818173131.png]]

![[Pasted image 20230818173153.png]]

![[Pasted image 20230818173247.png]]

![[Pasted image 20230818173319.png]]

![[Pasted image 20230818173343.png]]


把《SecureCrt.v.7.0-kg.exe》程序放在安装目录下

![[Pasted image 20230818173453.png]]


![[Pasted image 20230818173523.png]]


![[Pasted image 20230818173620.png]]

![[Pasted image 20230818173645.png]]

把以下图的信息记录下来：
![[Pasted image 20230818173854.png]]
ygeR
TEAM ZWT
03-62-009248
08-18-2023
ABE5RE HKAFEF 39HNAB TK25AD AAK8BF 6QXSZ5 WFNNH6 N52MK6

双击桌面的《SecureCRT 9.4》快捷键

![[Pasted image 20230818173933.png]]

不输入任何内容，下一步
![[Pasted image 20230818174907.png]]

![[Pasted image 20230818174930.png]]

![[Pasted image 20230818175018.png]]

![[Pasted image 20230818175105.png]]

#### 5.1.2 安装Xshell
 双击安装即可，不需要破解。

### 5.2 安装Xftp传输工具
双击安装即可，不需要破解。