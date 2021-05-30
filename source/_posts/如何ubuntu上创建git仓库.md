---
title: 如何ubuntu上创建git仓库
catalog: true
date: 2021-04-21 17:34:51
subtitle:
header-img:
tags: [Linux, git]
---

# 如何ubuntu上创建git仓库

### 服务端

- 安装git

```
[root@admin ~]# apt-get install -y git        #执行该命令进行Git安装
ziming@zm-gcp-vm-ubuntu16-tw-8:~$ git version
git version 2.7.4   # 查看是否安装成功
```

- linux服务器端创建新用户来管理git仓库

```
ziming@zm-gcp-vm-ubuntu16-tw-8:/home$ sudo useradd git
ziming@zm-gcp-vm-ubuntu16-tw-8:/home$ sudo passwd git
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
```

- 创建git仓库，并且将管理者变成新创建的git用户

```
#在git用户目录下创建仓库目录repositroy，并且创建gittest.git项目
ziming@zm-gcp-vm-ubuntu16-tw-8:/home$ sudo mkdir -p ./git/rep/gcp.git
ziming@zm-gcp-vm-ubuntu16-tw-8:/home$ ls 		#查看/home/目录下有哪些用户目录
git  jermainenee  me  ubuntu  ziming  zimingnee
ziming@zm-gcp-vm-ubuntu16-tw-8:/home$ cd git 	#进入git用户目录
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git$ ls	#查看git用户目录下有哪些目录/文件
rep
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git$ cd rep/	#进入repository仓库目录
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git/rep$ ls	#查看仓库目录下的项目目录
gcp.git

ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git/rep$ git init --bare ./gcp.git	#这步很重要，初始化项目测试目录
/home/git/rep/gcp.git/refs: Permission denied
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git/rep$ sudo git init --bare ./gcp.git
Initialized empty Git repository in /home/git/rep/gcp.git/
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git/rep$ cd ..

ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git$ ll #查看gittest.git项目文件夹的拥有者
total 12
drwxr-xr-x 3 root root 4096 Apr 21 19:01 ./
drwxr-xr-x 8 root root 4096 Apr 21 19:01 ../
drwxr-xr-x 3 root root 4096 Apr 21 19:01 rep/

ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git$ sudo chown -R git:git rep
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git$ ll 	#再次查看gittest.git项目文件夹的拥有者
total 12
drwxr-xr-x 3 root root 4096 Apr 21 19:01 ./
drwxr-xr-x 8 root root 4096 Apr 21 19:01 ../	
drwxr-xr-x 3 git  git  4096 Apr 21 19:01 rep/  	# 拥有者是git用户
```

- 安装SSH服务

```
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git$ sudo apt-get install openssh-server 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
openssh-server is already the newest version (1:7.2p2-4ubuntu2.10).
0 upgraded, 0 newly installed, 0 to remove and 13 not upgraded.
```

### 客户端

- 在client端新建文件夹拉取git项目

```
ziming@Mechrev MINGW64 /d/development/WebStormProjects
$ mkdir gcp

ziming@Mechrev MINGW64 /d/development/WebStormProjects
$ cd gcp/
```

- 本地拉取服务器项目

```
$ git clone git@35.236.130.209:22/:/home/git/rep/gcp.git
Cloning into 'gcp'...
git@35.236.130.209: Permission denied (publickey). # 无权限
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

### 解决权限问题创建sshkey

### 服务端

```
ziming@zm-gcp-vm-ubuntu16-tw-8:/etc/ssh$ sudo sshd_config
ziming@zm-gcp-vm-ubuntu16-tw-8:/etc/ssh$ sudo vim sshd_config
# 添加三行 如果已有注释就可以放开
PubkeyAuthentication yes
RSAAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys

ziming@zm-gcp-vm-ubuntu16-tw-8:/etc/ssh$ sudo sshd_config

ziming@zm-gcp-vm-ubuntu16-tw-8:/etc/ssh$ sudo vim sshd_config
ziming@zm-gcp-vm-ubuntu16-tw-8:/etc/ssh$ sudo service sshd restart # 重启

# 切换文件所有者
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git$ sudo chown -R git:git .ssh
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git$ ll
total 16
drwxr-xr-x 4 root root 4096 Apr 21 19:23 ./
drwxr-xr-x 8 root root 4096 Apr 21 19:01 ../
drwxr-xr-x 3 git  git  4096 Apr 21 19:01 rep/
drwxr-xr-x 2 git  git  4096 Apr 21 19:23 .ssh/
```

- 客户端导入公匙

```
cd ~/.ssh/id_rsa.pub
```

- 服务端导入

```
# 导入pub key
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git/.ssh$ sudo vim authorized_keys
# 查看是否导入
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git/.ssh$ cat authorized_keys 
sh-rsa 
# 修改权限
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git/.ssh$ sudo chmod 600 authorized_keys 

ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git/.ssh$ sudo chmod 700 ../.ssh/
ziming@zm-gcp-vm-ubuntu16-tw-8:/home/git/.ssh$
```

再次拉取即可

![image-20210530173736429](如何ubuntu上创建git仓库/image-20210530173736429.png)

> 在Ubuntu上搭建git服务器
> https://blog.csdn.net/baidu_38661691/article/details/88658033
