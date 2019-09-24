---
title: Docker学习10-部署数据库
catalog: true
date: 2019-09-24 09:19:36
subtitle:
header-img:
tags: [Docker]
---
# Docker部署数据库
容器化部署的持久化

## 拉取MySQL
~~~
➜  ~ docker pull mysql
Using default tag: latest
Got permission denied while trying to connect to the Dock/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.4=latest: dial unix /var/run/docker.sock: connect: permiss
➜  ~ sudo docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
8f91359f1fff: Pull complete
6bbb1c853362: Pull complete
e6e554c0af6f: Pull complete
f391c1a77330: Pull complete
414a8a88eabc: Pull complete
fee78658f4dd: Pull complete
9568f6bff01b: Pull complete
5a026d8bbe50: Pull complete
07f193b54ae1: Pull complete
1e404375a275: Pull complete
b81b2ef0e430: Pull complete
2f499f36bd40: Pull complete
Digest: sha256:6d95fa56e008425121e24d2c01b76ebbf51ca1df0bafb1edbe1a46937f4a149d
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
~~~

可以看到最新的mysql是8.0,这个版本取消了MyISAM 支持实务,效率低
支持InnoDB支持实务,效率也提高了
互联网企业的开发技能真正需要的技能,微服务架构,也就是分布式系统开发,数据库要支持分布式,目前的大多数企业是通过第三方方案支持,也就是互联网企业的需要原生分布式.
8.0的MySQL支持原生分布式数据库的解决方案

## 本博客使用5.7版本
5.7的mysql 支持NoSQL 效率方面是比之前版本快的.
`➜  ~ sudo docker pull mysql:5.7.22`

## 运行容器
如果这里直接启动mysql是启动的latest版本的mysql也就是8.0以上的.但是这样是启动不了的,因为最新版的启动方式已经改变.所有我们需要启动的是5.7版本的mysql.

~~~
docker run -p 3307:3306 --name mysql \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7.22
68161a1af1760cac2ca7eea5a5c71489b79b34eb834ca9cbae233b5c2daa2244
~~~

### 命令参数：

- p 3307:3306：将容器的 3306 端口映射到主机的 3307 端口
- v /usr/local/docker/mysql/conf:/etc/mysql：将主机当前目录下的 conf 挂载到容器的 /etc/mysql
- v /usr/local/docker/mysql/logs:/var/log/mysql：将主机当前目录下的 logs 目录挂载到容器的 /var/log/mysql
- v /usr/local/docker/mysql/data:/var/lib/mysql：将主机当前目录下的 data 目录挂载到容器的 /var/lib/mysql
- e MYSQL\_ROOT\_PASSWORD=root：初始化 root 用户的密码
- d 守护态运行
### ECS服务器开放端口
3306-3316开放10个端口供给docker容器使用
![](1.png)

### 查看容器启动情况
~~~
➜  ~ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
f352fd16f729        mysql:5.7.22        "docker-entrypoint.s…"   14 seconds ago      Up 13 seconds       0.0.0.0:3307->3306/tcp   mysql
e74b9cc8e192        tomcat              "catalina.sh run"        16 hours ago        Up 16 hours         0.0.0.0:8084->8080/tcp   volumeDemo1
a569649fcc5a        tomcat              "catalina.sh run"        17 hours ago        Up 17 hours         0.0.0.0:8083->8080/tcp   volumeDemo
d53272c36764        tomcat              "catalina.sh run"        19 hours ago        Up 19 hours         0.0.0.0:8082->8080/tcp   mytomcat2
849e5b576998        myproject           "catalina.sh run"        23 hours ago        Up 19 hours         0.0.0.0:8081->8080/tcp   elastic_panini
~~~
## 使用客户端工具连接 MySQL
测试连接成功

![](2.png)

查看数据库
![](3.png)

