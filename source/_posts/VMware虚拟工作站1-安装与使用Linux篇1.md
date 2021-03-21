---
title: "VMware虚拟工作站1-安装与使用Linux篇1"
catalog: true
tags: []
date: 2019-10-24 13:48:33
subtitle:
header-img:
---
# VMware虚拟工作站1-安装与使用Linux篇1
## 安装VMware
- 下载
    - VMwareworkstation64_15.1.0-13591040
    - ubuntu-18.04.2-live-server-amd64.iso

将VMware安装好
## 创建虚拟机
1. 点击创建虚拟机![1](VMware虚拟工作站1-安装与使用Linux篇1/1.png)
2. 点击自定义高级，然后下一步![2](VMware虚拟工作站1-安装与使用Linux篇1/2.png)
3. 硬件界面默认，点击下一步![3](VMware虚拟工作站1-安装与使用Linux篇1/3.png)
4. 新建虚拟机向导，选择稍后安装操作系统![4](VMware虚拟工作站1-安装与使用Linux篇1/4.png)
    如果这里选择光盘镜像会自动帮你安装，自动安装的会出问题。
5. 选择安装系统下一步，我的是64位下载的ubuntu发行版64![5](VMware虚拟工作站1-安装与使用Linux篇1/5.png)
6. 命名虚拟机的名字，然后在磁盘选择一个文件夹放置，然后下一步![6](VMware虚拟工作站1-安装与使用Linux篇1/6.png)
7. 选择处理器的数量，虚拟的![7](VMware虚拟工作站1-安装与使用Linux篇1/7.png)
8. 内存设置，虚拟的，我选的6GB![8](VMware虚拟工作站1-安装与使用Linux篇1/8.png)
9. 网络选择NAT![9](VMware虚拟工作站1-安装与使用Linux篇1/9.png)
10. I/O控制器，选择logic![10](VMware虚拟工作站1-安装与使用Linux篇1/10.png)
11. 硬盘类型，scsi![11](VMware虚拟工作站1-安装与使用Linux篇1/11.png)
12. 创建新虚拟磁盘，20G就够了![12](VMware虚拟工作站1-安装与使用Linux篇1/12.png)

拆分成多个以后创建虚拟机后，不会直接占用20GB空间，随着虚拟机内容变多后，所占内容会才会慢慢把空间扩大。

![13](VMware虚拟工作站1-安装与使用Linux篇1/13.png)

下一步
![14](VMware虚拟工作站1-安装与使用Linux篇1/14.png)

13. 浏览创建参数
![15](VMware虚拟工作站1-安装与使用Linux篇1/15.png)
14. 点击完成后
![16](VMware虚拟工作站1-安装与使用Linux篇1/16.png)

## 启动ubuntu虚拟机
- 点击运行启动，出现弹框，点击确定。
![17](VMware虚拟工作站1-安装与使用Linux篇1/17.png)
- 因为没有插光驱，所以显示没有
![18](VMware虚拟工作站1-安装与使用Linux篇1/18.png)
- 点击停止，然后编辑虚拟机
![19](VMware虚拟工作站1-安装与使用Linux篇1/19.png)![20](VMware虚拟工作站1-安装与使用Linux篇1/20.png)
- 弹出对话框，然后选择CD/DVD
![21](VMware虚拟工作站1-安装与使用Linux篇1/21.png)
- 将iso文件导入后启动Ubuntu
![22](VMware虚拟工作站1-安装与使用Linux篇1/22.png)![23](VMware虚拟工作站1-安装与使用Linux篇1/23.png)
- 启动加载ubuntu
![24](VMware虚拟工作站1-安装与使用Linux篇1/24.png)
- 语言选择,选择english,为防止字符编码bug
![25](VMware虚拟工作站1-安装与使用Linux篇1/25.png)![26](VMware虚拟工作站1-安装与使用Linux篇1/26.png)
- 选择install ubuntu
![27](VMware虚拟工作站1-安装与使用Linux篇1/27.png)
- 网络设置这里我选择DHCP自动分配
![28](VMware虚拟工作站1-安装与使用Linux篇1/28.png)![29](VMware虚拟工作站1-安装与使用Linux篇1/29.png)![30](VMware虚拟工作站1-安装与使用Linux篇1/30.png)
- 选择代理,直接选DONE
![31](VMware虚拟工作站1-安装与使用Linux篇1/31.png)
- 选择镜像地址,这里选择阿里云镜像,后面系统会安装东西.这样会快些.
`http://mirrors.aliyun.com/ubuntu`
![32](VMware虚拟工作站1-安装与使用Linux篇1/32.png)
- 磁盘挂载,我选择的LVM
    - Use An entire Disk-使用整个磁盘
    - Use An Entire Disk And Set Up LVM-使用整个磁盘并设置
    - Manual-手动
    - Back-返回
LVM是磁盘扩容技术，一定要选择LVM
    500GB C /path
    500GB D /path
    如果没有需那种LVM那么扩容以后无法合并挂载点
![33](VMware虚拟工作站1-安装与使用Linux篇1/33.png)![34](VMware虚拟工作站1-安装与使用Linux篇1/34.png)
- Filesystem setup   文件系统设置
    - FILE SYSTEM SUMMARY    文件系统摘要
    - No disk or partitions mounted   未安装磁盘或分区
    - AVAILABLE DEVICES  可用设备
    ![35](VMware虚拟工作站1-安装与使用Linux篇1/35.png)![36](VMware虚拟工作站1-安装与使用Linux篇1/36.png)
- 设置用户名,服务器名称
![37](VMware虚拟工作站1-安装与使用Linux篇1/37.png)![38](VMware虚拟工作站1-安装与使用Linux篇1/38.png)
- 默认安装openssh
![39](VMware虚拟工作站1-安装与使用Linux篇1/39.png)
- 默认安装插件,不安装
![40](VMware虚拟工作站1-安装与使用Linux篇1/40.PNG)
- 选择重启
接着是一个安装过程，请耐心等待，安装完成后会在 [View full log] 按钮下多出一个 [Reboot Now] 按钮。
![41](VMware虚拟工作站1-安装与使用Linux篇1/41.png)![42](VMware虚拟工作站1-安装与使用Linux篇1/42.png)

## 成功启动
![43](VMware虚拟工作站1-安装与使用Linux篇1/43.png)

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
## 安装zsh 和 oh-my-zsh
1. 安装 zsh
`apt install zsh -y`
zsh 设为默认 shell
chsh -s /bin/zsh

2. 安装 oh-my-zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
或
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

3. 插件
安装 incr 自动补全插件推荐

cd ~/.oh-my-zsh/plugins/
mkdir incr && cd incr
wget http://mimosa-pudica.net/src/incr-0.2.zsh
vi ~/.zshrc
插入下面这条
~~~
source ~/.oh-my-zsh/plugins/incr/incr*.zsh
~~~
source ~/.zshrc

## 参考资料
> https://blog.csdn.net/ruo_62/article/details/90233501
