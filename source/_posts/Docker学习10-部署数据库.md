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
## 查看docker目录下的文件
~~~
➜  docker ls
mysql  tomcat
➜  mysql ls
conf  data  logs
➜  mysql conf
➜  conf ls
➜  conf cd ..
➜  mysql cd data
➜  data ls
auto.cnf          ib_buffer_pool  performance_schema
ca-key.pem        ibdata1         private_key.pem
ca.pem            ib_logfile0     public_key.pem
client-cert.pem   ib_logfile1     server-cert.pem
client-key.pem    ibtmp1          server-key.pem
f352fd16f729.pid  mysql           sys
~~~

docker帮我创建了mysql文件夹,和window和linux下的目录一样的.
### 一点小问题
- 以交互方式启动docker
~~~
➜  data sudo docker run -it --rm mysql:5.7.22 bash
root@fc1072850e62:/# 
➜  data sudo docker run -it --rm mysql:5.7.22 bash
root@fc1072850e62:/# ls
bin			    etc    mnt	 sbin  var
boot			    home   opt	 srv
dev			    lib    proc  sys
docker-entrypoint-initdb.d  lib64  root  tmp
entrypoint.sh		    media  run	 usr
root@fc1072850e62:/# whereis mysql
mysql: /usr/bin/mysql /usr/lib/mysql /etc/mysql /usr/share/mysql
root@fc1072850e62:/# cd /etc/mysql/
root@fc1072850e62:/etc/mysql# ls
conf.d	my.cnf.fallback  mysql.conf.d
my.cnf	mysql.cnf
root@fc1072850e62:/etc/mysql# ll

# 查看配置文件
root@fc1072850e62:/etc/mysql# cd mysql.conf.d/
root@fc1072850e62:/etc/mysql/mysql.conf.d# ls
mysqld.cnf
root@fc1072850e62:/etc/mysql/mysql.conf.d# cat mysqld.cnf
# 配置文件详细
[mysqld]
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
datadir		= /var/lib/mysql
#log-error	= /var/log/mysql/error.log
# By default we only accept connections from localhost
#bind-address	= 127.0.0.1
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
root@fc1072850e62:/etc/mysql/mysql.conf.d# 
~~~
- 之前启动时候的参数
~~~
docker run -p 3307:3306 --name mysql \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7.22
68161a1af1760cac2ca7eea5a5c71489b79b34eb834ca9cbae233b5c2daa2244
~~~
与启动后的mysql的配置文件一对比,就发现相同之处了.

## 导入数据库数据

mysql默认情况下可导入的数据库大小为: 16MB
~~~
root@fc1072850e62:/etc/mysql# ls -a
.   conf.d  my.cnf.fallback  mysql.conf.d
..  my.cnf  mysql.cnf
root@fc1072850e62:/etc/mysql# cd conf.d/
root@fc1072850e62:/etc/mysql/conf.d# ls -al
total 20
drwxr-xr-x 2 root root 4096 Jul 27  2018 .
drwxr-xr-x 4 root root 4096 Jul 27  2018 ..
-rw-r--r-- 1 root root   43 Jul 27  2018 docker.cnf
-rw-r--r-- 1 root root    8 Jul  9  2016 mysql.cnf
-rw-r--r-- 1 root root   55 Jul  9  2016 mysqldump.cnf
root@fc1072850e62:/etc/mysql/conf.d# cat mysqldump.cnf
[mysqldump]
quick
quote-names
# 数据库初始时可设置的文件大小
max_allowed_packet	= 16M
root@fc1072850e62:/etc/mysql/conf.d#
~~~

实际上需要把`max_allowed_packet      = 16M` 这个配置放到
~~~
root@fc1072850e62:/etc/mysql/mysql.conf.d# ls
mysqld.cnf
~~~
mysql.cnf下.

但是这个容器如果删除掉,这个配置文件也会消失,所以需要使用数据卷将这个配置文件做成共享文件.这样就算这个mysql容器被销毁,下一次新建mysql容器时也会共享数据卷里面的mysql.cnf文件

## 数据卷共享mysql.cnf
~~~
➜  mysql cd conf
➜  conf pwd
/usr/local/docker/mysql/conf
➜  conf ls
➜  conf ll
total 0
~~~
退出容器后进入conf目录发现没有任何文件,需要拉取配置文件到这里

### 停止删除原先的容器
~~~
➜  conf sudo docker ps
[sudo] password for ziming:
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
f352fd16f729        mysql:5.7.22        "docker-entrypoint.s…"   25 minutes ago      Up 25 minutes       0.0.0.0:3307->3306/tcp   mysql
e74b9cc8e192        tomcat              "catalina.sh run"        17 hours ago        Up 17 hours         0.0.0.0:8084->8080/tcp   volumeDemo1
a569649fcc5a        tomcat              "catalina.sh run"        17 hours ago        Up 17 hours         0.0.0.0:8083->8080/tcp   volumeDemo
d53272c36764        tomcat              "catalina.sh run"        19 hours ago        Up 19 hours         0.0.0.0:8082->8080/tcp   mytomcat2
849e5b576998        myproject           "catalina.sh run"        23 hours ago        Up 19 hours         0.0.0.0:8081->8080/tcp   elastic_panini
➜  conf sudo docker stop f352
f352
➜  conf sudo docker rm f352
f352
~~~

### 修改原先启动配置
~~~
docker run -p 3307:3306 --name mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7.22
~~~

**notice**:
交互的方式启动容器. run -it
交互的方式进入容器. exec -it

~~~
➜  conf sudo docker run -p 3307:3306 --name mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7.22
2fbdec92f87ac3c9fb53801476f31fa1a0a66c31a050f8138daa4d5614e886cf
➜  conf sudo docker exec -it mysql bash
root@2fbdec92f87a:/# cd /etc/mysql/mysql.conf.d/
root@2fbdec92f87a:/etc/mysql/mysql.conf.d# ls -la
total 12
drwxr-xr-x 2 root root 4096 Jul 27  2018 .
drwxr-xr-x 4 root root 4096 Jul 27  2018 ..
-rw-r--r-- 1 root root 1191 Jul 27  2018 mysqld.cnf
~~~

### 将文件限制大小追加进次配置文件

~~~
root@2fbdec92f87a:/etc/mysql/mysql.conf.d# echo "max_allowed_packet      = 128M" >> mysqld.cnf
root@2fbdec92f87a:/etc/mysql/mysql.conf.d# cat mysqld.cnf

[mysqld]
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
datadir		= /var/lib/mysql
#log-error	= /var/log/mysql/error.log
# By default we only accept connections from localhost
#bind-address	= 127.0.0.1
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
max_allowed_packet      = 128M
~~~
追加成功

### 重启docker
~~~
➜  conf sudo docker restart mysql
mysql
➜  conf
~~~~

### 导入数据库
略

### 将容器的文件复制到宿主机
- 新建一个连接进入容器
~~~
[root@MyCentOS]~# docker exec -it mysql bash
root@2fbdec92f87a:/# cd /e
entrypoint.sh  etc/
root@2fbdec92f87a:/# cd /etc/mysql/
root@2fbdec92f87a:/etc/mysql# ls -al
total 36
drwxr-xr-x 1 root root 4096 Jul 27  2018 .
drwxr-xr-x 1 root root 4096 Sep 24 02:34 ..
drwxr-xr-x 2 root root 4096 Jul 27  2018 conf.d
lrwxrwxrwx 1 root root   24 Jul 27  2018 my.cnf -> /etc/alternatives/my.cnf
-rw-r--r-- 1 root root  839 Jul  9  2016 my.cnf.fallback
-rw-r--r-- 1 root root  796 Mar  4  2018 mysql.cnf
drwxr-xr-x 1 root root 4096 Jul 27  2018 mysql.conf.d
~~~

- 将容器的文件拷贝出来
`sudo docker cp mysql:/etc/mysql .`
此时查看拷贝文件已经存在
~~~
➜  conf ls
mysql
➜  conf cd mysql
➜  mysql ls
conf.d  my.cnf.fallback  mysql.conf.d
my.cnf  mysql.cnf
➜  mysql
# 删除mysql文件夹
➜  conf sudo rm -fr mysql
➜  conf ls
conf.d  my.cnf.fallback  mysql.conf.d
my.cnf  mysql.cnf
# 退回上一级
➜  conf cd ..
# 停掉mysql容器

mysql sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
2fbdec92f87a        mysql:5.7.22        "docker-entrypoint.s…"   14 minutes ago      Up 9 minutes        0.0.0.0:3307->3306/tcp   mysql
e74b9cc8e192        tomcat              "catalina.sh run"        17 hours ago        Up 17 hours         0.0.0.0:8084->8080/tcp   volumeDemo1
a569649fcc5a        tomcat              "catalina.sh run"        17 hours ago        Up 17 hours         0.0.0.0:8083->8080/tcp   volumeDemo
d53272c36764        tomcat              "catalina.sh run"        20 hours ago        Up 20 hours         0.0.0.0:8082->8080/tcp   mytomcat2
849e5b576998        myproject           "catalina.sh run"        24 hours ago        Up 20 hours         0.0.0.0:8081->8080/tcp   elastic_panini
➜  mysql sudo docker stop 2
2
➜  mysql sudo docker rm 2  
2
~~~
### 运行带着数据卷的配置命令 
~~~
docker run -p 3307:3306 --name mysql \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7.22
~~~
- 再进入/etc/mysql/mysql.conf.d查看文件

~~~
➜  mysql docker run -p 3307:3306 --name mysql \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7.22
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/create?name=mysql: dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
➜  mysql sudo docker run -p 3307:3306 --name mysql \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7.22
00e3d56be3658b26e11ec7d045843638fcba8a938aa2877bd8f9f6e2aa950408
➜  mysql sudo docker exec -it mysql bash
root@00e3d56be365:/# cd /etc/mysql/
root@00e3d56be365:/etc/mysql# ls -la
total 24
drwxr-xr-x 4 root root 4096 Sep 24 02:46 .
drwxr-xr-x 1 root root 4096 Sep 24 02:52 ..
drwxr-xr-x 2 root root 4096 Jul 27  2018 conf.d
lrwxrwxrwx 1 root root   24 Jul 27  2018 my.cnf -> /etc/alternatives/my.cnf
-rw-r--r-- 1 root root  839 Jul  9  2016 my.cnf.fallback
-rw-r--r-- 1 root root  796 Mar  4  2018 mysql.cnf
drwxr-xr-x 2 root root 4096 Jul 27  2018 mysql.conf.d
root@00e3d56be365:/etc/mysql# cd mysql.conf.d/
root@00e3d56be365:/etc/mysql/mysql.conf.d# ls -la
total 12
drwxr-xr-x 2 root root 4096 Jul 27  2018 .
drwxr-xr-x 4 root root 4096 Sep 24 02:46 ..
-rw-r--r-- 1 root root 1222 Sep 24 02:36 mysqld.cnf
root@00e3d56be365:/etc/mysql/mysql.conf.d# cat mysqld.cnf

[mysqld]
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
datadir		= /var/lib/mysql
#log-error	= /var/log/mysql/error.log
# By default we only accept connections from localhost
#bind-address	= 127.0.0.1
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
max_allowed_packet      = 128M
root@00e3d56be365:/etc/mysql/mysql.conf.d#
~~~
发现配置变化了,追加进去的数据成功.

- 查看宿主机上的数据卷
就算删除了原先的容器,但是里面的数据库数据并不会被删除,因为这些数据都存放在数据卷里面的,数据卷是生效的
~~~
➜  mysql ls
conf  data  logs
➜  mysql
~~~

以上为数据卷的数据库容器化部署

### 参考资料
> https://www.bilibili.com/video/av29384041/?p=42
> https://www.funtl.com/zh/docker/Docker-%E6%9E%84%E5%BB%BA-MySQL.html



