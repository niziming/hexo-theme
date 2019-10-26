---
title: "VMware虚拟工作站00-安装与使用Linux篇00"
catalog: true
tags: []
date: 2019-10-24 13:48:33
subtitle:
header-img:
---
# VMware虚拟工作站00-安装与使用Linux篇00
## 安装VMware
- 下载
    - VMwareworkstation64_15.1.0-13591040
    - ubuntu-18.04.2-live-server-amd64.iso

将VMware安装好
## 创建虚拟机
1. 点击创建虚拟机
![](1.png)
2. 点击自定义高级，然后下一步
![](2.png)
3. 硬件界面默认，点击下一步
![](3.png)
4. 新建虚拟机向导，选择稍后安装操作系统
如果这里选择光盘镜像会自动帮你安装，自动安装的会出问题。
![](4.png)
5. 选择安装系统下一步，我的是64位下载的ubuntu发行版64
![](5.png)
6. 命名虚拟机的名字，然后在磁盘选择一个文件夹放置，然后下一步
![](6.png)
7. 选择处理器的数量，虚拟的
![](7.png)
8. 内存设置，虚拟的，我选的6GB
![](8.png)
9. 网络选择NAT
![](9.png)
10. I/O控制器，选择logic
![](10.png)
11. 硬盘类型，scsi
![](11.png)
12. 创建新虚拟磁盘，20G就够了
![](12.png)
拆分成多个以后创建虚拟机后，不会直接占用20GB空间，随着虚拟机内容变多后，所占内容会才会慢慢把空间扩大。
![](13.png)
下一步
![](14.png)
13. 浏览创建参数
![](15.png)
14. 点击完成后
![](16.png)

## 启动ubuntu虚拟机
- 点击运行启动，出现弹框，点击确定。
![](17.png)
- 因为没有插光驱，所以显示没有
![](18.png)
- 点击停止，然后编辑虚拟机
![](19.png)
![](20.png)
- 弹出对话框，然后选择CD/DVD
![](21.png)
- 将iso文件导入后启动Ubuntu
![](22.png)
![](23.png)
- 启动加载ubuntu
![](24.png)
- 语言选择,选择english,为防止字符编码bug
![](25.png)
![](26.png)
- 选择install ubuntu
![](27.png)
- 网络设置这里我选择DHCP自动分配
![](28.png)
![](29.png)
![](30.png)
- 选择代理,直接选DONE
![](31.png)
- 选择镜像地址,这里选择阿里云镜像,后面系统会安装东西.这样会快些.
`http://mirrors.aliyun.com/ubuntu`
![](32.png)
- 磁盘挂载,我选择的LVM
    - Use An entire Disk-使用整个磁盘
    - Use An Entire Disk And Set Up LVM-使用整个磁盘并设置
    - Manual-手动
    - Back-返回
LVM是磁盘扩容技术，一定要选择LVM
    500GB C /path
    500GB D /path
    如果没有需那种LVM那么扩容以后无法合并挂载点
![](33.png)
![](34.png)
    - Filesystem setup   文件系统设置
    - FILE SYSTEM SUMMARY    文件系统摘要
    - No disk or partitions mounted   未安装磁盘或分区
    - AVAILABLE DEVICES  可用设备
![](35.png)
![](36.png)
- 设置用户名,服务器名称
![](37.png)
![](38.png)
- 默认安装openssh
![](39.png)
- 默认安装插件,不安装
![](40.png)
- 选择重启
接着是一个安装过程，请耐心等待，安装完成后会在 [View full log] 按钮下多出一个 [Reboot Now] 按钮。
![](41.png)
![](42.png)

## 成功启动
![](43.png)

## Ubuntu server 设置 root 密码
ubuntu server 安装的时候要你新建一个用户，安装完成后，你需要手动开启 root。
`sudo passwd root`
根据提示依次输入:
1. 输入你当前用户的密码
2. 输入你希望的 root 用户的密码
3. 确认密码

设置完成。

## 安装ssh
### 检查软件是否安装
~~~
apt-cache policy openssh-client openssh-server
~~~
### 安装服务端
~~~
apt-get install openssh-server
~~~
### 安装客户端
~~~
apt-get install openssh-client
~~~

应该是sshd的设置不允许root用户用密码远程登录
 
1. 修改 vim /etc/ssh/sshd_config
 
找到# Authentication:
~~~
LoginGraceTime 120
PermitRootLogin without passwd
StrictModes yes
~~~
改成
~~~
# Authentication:
LoginGraceTime 120
PermitRootLogin yes
StrictModes yes
~~~
2. 重启ssh

`systemctl restart sshd`

## 参考资料
> https://blog.csdn.net/ruo_62/article/details/90233501
