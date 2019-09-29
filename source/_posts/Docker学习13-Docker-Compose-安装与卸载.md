---
title: Docker学习13-Docker-Compose-安装与卸载
catalog: true
tags: []
date: 2019-09-29 09:32:51
subtitle:
header-img:
---
# Docker Compose安装与卸载
Compose 支持 Linux、macOS、Windows 10 三大平台。

Compose 可以通过 Python 的包管理工具 pip 进行安装，也可以直接下载编译好的二进制文件使用，甚至能够直接在 Docker 容器中运行。

前两种方式是传统方式，适合本地环境下安装使用；最后一种方式则不破坏系统环境，更适合云计算场景。

Docker for Mac 、Docker for Windows 自带 docker-compose 二进制文件，安装 Docker 之后可以直接使用。
~~~
$ docker-compose --version

docker-compose version 1.17.1, build 6d101fb
~~~
Linux 系统请使用以下介绍的方法安装.

## 二进制包
在linux上的安装,从官方github release处下载编译好的二进制文件
我本人使用的CentOS进行安装,包管理器为yum,所以要对yum进行一些配置

## yum仓库添加
~~~
sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
~~~
执行结果
~~~
[root@MyCentOS]/etc# sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
heredocd> [dockerrepo]
heredocd> name=Docker Repository
heredocd> baseurl=https://yum.dockerproject.org/repo/main/centos/7/
heredocd> enabled=1
heredocd> gpgcheck=1
heredocd> gpgkey=https://yum.dockerproject.org/gpg
heredocd> EOF
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
~~~
## 使用 Docker 国内镜像（为 Docker 镜像下载提速，非必须）
使用 https://registry.docker-cn.com
或者  https://get.daocloud.io
~~~
[root@MyCentOS]/etc# curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://fe8a7d6e.m.daoc
loud.io
docker version >= 1.12
{
 "registry-mirrors": ["http://fe8a7d6e.m.daocloud.io"]
}
Success.
You need to restart docker to take effect: sudo systemctl restart docker
~~~

## 安装docker-compose

`
$ sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
~~~
[root@MyCentOS]~# curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   617    0   617    0     0    923      0 --:--:-- --:--:-- --:--:--   923
100 11.2M  100 11.2M    0     0   634k      0  0:00:18  0:00:18 --:--:--  396k

~~~

~~~
[root@MyCentOS]/etc# docker-compose -v
docker-compose version 1.22.0, build f46880fe
~~~
安装成功

## 赋予权限
为用户组和其他用户的权限赋予
`$ sudo chmod +x /usr/local/bin/docker-compose`
~~~
root@MyCentOS]/usr/local/bin# ll
total 11488
-rwxr-xr-x 1 root root      225 Sep 18 16:08 chardetect
-rwxr-xr-x 1 root root 11750136 Sep 29 10:05 docker-compose
-rwxr-xr-x 1 root root      209 Sep 18 16:45 pytest
-rwxr-xr-x 1 root root      209 Sep 18 16:45 py.test
~~~

## 参考资料
> https://www.cnblogs.com/lywJ/p/10716062.html
> https://www.funtl.com/zh/docker-compose/Docker-Compose-%E5%AE%89%E8%A3%85%E4%B8%8E%E5%8D%B8%E8%BD%BD.html
