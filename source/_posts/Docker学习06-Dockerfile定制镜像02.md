---
title: Docker学习06-Dockerfile定制镜像02
catalog: true
date: 2019-09-19 14:23:53
subtitle:
header-img:
tags: [Docker, Dockerfile]
---

## Docker学习06-Dockerfile定制镜像02

- Docker 停止一个运行的容器
`docker stop <id>`
~~~
7db5c8940e5a        tomcat              "catalina.sh run"        29 minutes ago       Up 29 minutes               0.0.0.0:8081->8080/tcp   dreamy_moore
[root@MyCentOS]~# docker rm 898
898
[root@MyCentOS]~# docker stop 7db
7db
[root@MyCentOS]~#
~~~

## 自己配置镜像
这个是用户经常操作的目录所有把文件存放在这里,所有和docker相关的都在里面
~~~
[root@MyCentOS]~# cd /usr/local
[root@MyCentOS]/usr/local#
[root@MyCentOS]/usr/local# mkdir docker
[root@MyCentOS]/usr/local# cd docker
[root@MyCentOS]/usr/local/docker# mkdir tomcat
[root@MyCentOS]/usr/local/docker# cd tomcat
[root@MyCentOS]/usr/local/docker/tomcat# vi Dockerfile
~~~

这个 Dockerfile 很简单，一共就两行。涉及到了两条指令，FROM 和 RUN
# Dockerfile脚本开始
~~~
FROM tomcat
RUN echo '<h1>Hello, Docker!</h1>' > /usr/local/tomcat/webapps/ROOT/index.jsp
~                                                                                        
~                                                                                           
~ 
~~~
## FROM指定基础镜像
所谓定制镜像，那一定是以一个镜像为基础，在其上进行定制。

## RUN 执行命令
RUN执行命令行命令的命令

所有的Dockerfile第一行都必须是FROM命令,RUN 指令在定制镜像时是最常用的指令之一
- shell格式:`RUN`就像直接在命令行中输入的命令一样。刚才写的 Dockerfile 中的 RUN 指令就是这种格式。
~~~
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.jsp
~~~

- exec 格式：RUN ["可执行文件", "参数1", "参数2"]，这更像是函数调用中的格式

既然 RUN 就像 Shell 脚本一样可以执行命令，那么我们是否就可以像 Shell 脚本一样把每个命令对应一个 RUN 呢？比如这样：

Dockerfile 中每一个指令都会建立一层，RUN 也不例外。每一个 RUN 的行为，就和刚才我们手工建立镜像的过程一样：新建立一层，在其上执行这些命令，执行结束后，commit 这一层的修改，构成新的镜像。

而上面的这种写法，创建了 7 层镜像。这是完全没有意义的，而且很多运行时不需要的东西，都被装进了镜像里，比如编译环境、更新的软件包等等。结果就是产生非常臃肿、非常多层的镜像，不仅仅增加了构建部署的时间，也很容易出错。 这是很多初学 Docker 的人常犯的一个错误。

Union FS 是有最大层数限制的，比如 AUFS，曾经是最大不得超过 42 层，现在是不得超过 127 层。

~~~
FROM debian:jessie

RUN apt-get update
RUN apt-get install -y gcc libc6-dev make
RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz"
RUN mkdir -p /usr/src/redis
RUN tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1
RUN make -C /usr/src/redis
RUN make -C /usr/src/redis install
~~~

上面的 Dockerfile 正确的写法应该是这样：
~~~
FROM debian:jessie

RUN buildDeps='gcc libc6-dev make' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps
~~~

以交互的方式启动一条命令
`~ sudo docker run -it --rm tomcat bash`
~~~
➜  ~ sudo docker run -it --rm tomcat bash
root@14630cb2f338:/usr/local/tomcat# ls -al
total 160
drwxr-sr-x 1 root staff  4096 Aug 22 00:26 .
drwxrwsr-x 1 root staff  4096 Aug 15 05:20 ..
-rw-r--r-- 1 root root  19534 Aug 14 22:27 BUILDING.txt
-rw-r--r-- 1 root root   5407 Aug 14 22:27 CONTRIBUTING.md
-rw-r--r-- 1 root root  57011 Aug 14 22:27 LICENSE
-rw-r--r-- 1 root root   1726 Aug 14 22:27 NOTICE
-rw-r--r-- 1 root root   3255 Aug 14 22:27 README.md
-rw-r--r-- 1 root root   7139 Aug 14 22:27 RELEASE-NOTES
-rw-r--r-- 1 root root  16262 Aug 14 22:27 RUNNING.txt
drwxr-xr-x 2 root root   4096 Aug 22 00:26 bin
drwxr-sr-x 2 root root   4096 Aug 14 22:27 conf
drwxr-sr-x 2 root staff  4096 Aug 22 00:26 include
drwxr-xr-x 2 root root   4096 Aug 22 00:26 lib
drwxrwxrwx 2 root root   4096 Aug 14 22:24 logs
drwxr-sr-x 3 root staff  4096 Aug 22 00:26 native-jni-lib
drwxr-xr-x 2 root root   4096 Aug 22 00:26 temp
drwxr-xr-x 7 root root   4096 Aug 14 22:24 webapps
drwxrwxrwx 2 root root   4096 Aug 14 22:24 work

root@14630cb2f338:/usr/local/tomcat# cd webapps/ROOT/
root@14630cb2f338:/usr/local/tomcat/webapps/ROOT# ls
RELEASE-NOTES.txt  bg-button.png  bg-upper.png	tomcat-power.gif  tomcat.png
WEB-INF		   bg-middle.png  favicon.ico	tomcat.css	  tomcat.svg
asf-logo-wide.svg  bg-nav.png	  index.jsp	tomcat.gif
root@14630cb2f338:/usr/local/tomcat/webapps/ROOT#

root@14630cb2f338:/usr/local/tomcat/webapps/ROOT# pwd
/usr/local/tomcat/webapps/ROOT
~~~
查看脚本
~~~
[root@MyCentOS]/usr/local/docker/tomcat# cat Dockerfile
## Dockerfile脚本开始
FROM tomcat
RUN echo '<h1>Hello, Docker!</h1>' > /usr/local/tomcat/webapps/ROOT/index.jsp
~~~
## 构建镜像
`docker build`



