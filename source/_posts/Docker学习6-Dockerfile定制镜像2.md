---
title: Docker学习6-Dockerfile定制镜像2
catalog: true
date: 2019-09-19 14:23:53
subtitle:
header-img:
tags: [Docker, Dockerfile]
---

## Docker学习6-Dockerfile定制镜像2

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

- 以交互的方式启动一条命令
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
RUN echo '<h1>Hello, Docker!</h1>' > /usr/local/tomcat/webapps/ROOT/index.html
~~~
## 构建镜像
- 在 Dockerfile 文件所在目录执行：
`docker build [选项] <上下文路径/URL/->`
`docker build -t <name> .`
`docker build -t <name> <Dockerfile>`

~~~
[root@MyCentOS]/usr/local/docker/tomcat# docker build -t mytomcatdocker .
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM tomcat
 ---> 96c4e536d0eb
Step 2/2 : RUN echo '<h1>Hello, Docker!</h1>' > /usr/local/tomcat/webapps/ROOT/index.html
 ---> Running in 5d88c8eacd22
Removing intermediate container 5d88c8eacd22
 ---> 12aa14e2403d
Successfully built 12aa14e2403d
Successfully tagged mytomcatdocker:latest
[root@MyCentOS]/usr/local/docker/tomcat#
~~~
RUN指令启动了一个容器 5d88c8eacd22 执行了所要求的命令，并最后提交了这一层 12aa14e2403d 随后删除了 5d88c8eacd22

- 查看镜像
~~~
➜  ~ sudo docker images
[sudo] password for ziming:
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mytomcatdocker      latest              12aa14e2403d        6 minutes ago       506MB
1.16.1              latest              97423576f288        3 weeks ago         126MB
tomcat              latest              96c4e536d0eb        4 weeks ago         506MB
nginx               1.16.1              41e97992fcd8        5 weeks ago         126MB
ubuntu              16.04               5e13f8dd4c1a        8 weeks ago         120MB
~~~

- 再以交互的方式进入mytomcatdocker
~~~
~ sudo docker run -it mytomcatdocker bash
root@8de78a99decd:/usr/local/tomcat# ls -al
~~~
- 查看文件
~~~
root@8de78a99decd:/usr/local/tomcat# ls -al
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
drwxr-xr-x 1 root root   4096 Aug 14 22:24 webapps
drwxrwxrwx 2 root root   4096 Aug 14 22:24 work
~~~
- 进入root
~~~
root@8de78a99decd:/usr/local/tomcat# cd webapps/ROOT/
root@8de78a99decd:/usr/local/tomcat/webapps/ROOT#
root@8de78a99decd:/usr/local/tomcat/webapps/ROOT# ls -al
total 196
drwxr-xr-x 1 root root  4096 Sep 20 01:34 .
drwxr-xr-x 1 root root  4096 Aug 14 22:24 ..
-rw-r--r-- 1 root root  7139 Aug 14 22:27 RELEASE-NOTES.txt
drwxr-xr-x 2 root root  4096 Aug 22 00:26 WEB-INF
-rw-r--r-- 1 root root 27235 Aug 14 22:27 asf-logo-wide.svg
-rw-r--r-- 1 root root   713 Aug 14 22:24 bg-button.png
-rw-r--r-- 1 root root  1918 Aug 14 22:24 bg-middle.png
-rw-r--r-- 1 root root  1401 Aug 14 22:24 bg-nav.png
-rw-r--r-- 1 root root  3103 Aug 14 22:24 bg-upper.png
-rw-r--r-- 1 root root 21630 Aug 14 22:24 favicon.ico
-rw-r--r-- 1 root root    24 Sep 20 01:34 index.html
-rw-r--r-- 1 root root 12208 Aug 14 22:27 index.jsp
-rw-r--r-- 1 root root  2376 Aug 14 22:24 tomcat-power.gif
-rw-r--r-- 1 root root  5581 Aug 14 22:27 tomcat.css
-rw-r--r-- 1 root root  2066 Aug 14 22:24 tomcat.gif
-rw-r--r-- 1 root root  5103 Aug 14 22:24 tomcat.png
-rw-r--r-- 1 root root 67795 Aug 14 22:27 tomcat.svg
~~~
index.html存在了
- 查看index.html
~~~
root@8de78a99decd:/usr/local/tomcat/webapps/ROOT# cat index.html
<h1>Hello, Docker!</h1>
~~~
## 删除index.jsp
但是我不想要index.jsp,所以需要在dockerfile脚本中删除ROOT下所有的东西这样,我暴露8081端口的时候只能看到index.html中的内容

- 进入Dockerfile
~~~
➜  ~ cd /usr/local/docker/tomcat
➜  tomcat ls
Dockerfile
➜  tomcat
~~~
- 修改Dockerfile
~~~
# Dockerfile脚本开始
FROM tomcat
RUN cd /usr/local/tomcat/webapps/ROOT/
RUN rm -fr *
RUN echo '<h1>Hello, Docker!</h1>' > /usr/local/tomcat/webapps/ROOT/index.html
~~~

- 重复先前的操作
删除掉原先创建的镜像
sudo docker image rm 12a

~~~
➜  tomcat sudo docker build -t mydocker .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM tomcat
 ---> 96c4e536d0eb
Step 2/4 : RUN cd /usr/local/tomcat/webapps/ROOT/
 ---> Running in 582adfa700d2
Removing intermediate container 582adfa700d2
 ---> 300e7b9c024b
Step 3/4 : RUN rm -fr *
 ---> Running in 0637d8d5df11
Removing intermediate container 0637d8d5df11
 ---> 137fe42507bf
Step 4/4 : RUN echo '<h1>Hello, Docker!</h1>' > /usr/local/tomcat/webapps/ROOT/index.html
 ---> Running in 55a7cef531e0
/bin/sh: 1: cannot create /usr/local/tomcat/webapps/ROOT/index.html: Directory nonexistent
The command '/bin/sh -c echo '<h1>Hello, Docker!</h1>' > /usr/local/tomcat/webapps/ROOT/index.html' returned a non-zero code: 2
➜  tomcat docker images
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/images/json: dial unix /var/run/docker.sock: connect: permission denied
➜  tomcat sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
<none>              <none>              137fe42507bf        About a minute ago   506MB
1.16.1              latest              97423576f288        3 weeks ago          126MB
tomcat              latest              96c4e536d0eb        4 weeks ago          506MB
nginx               1.16.1              41e97992fcd8        5 weeks ago          126MB
ubuntu              16.04               5e13f8dd4c1a        8 weeks ago          120MB
➜  tomcat
~~~
在创建过程中`RUN rm -rf *` 连着ROOT也一起删除了所有会报错,所以新的镜像成为了虚悬镜像

- 修改脚本
~~~ linux
# Dockerfile脚本开始
FROM tomcat
WORKDIR /usr/local/tomcat/webapps/ROOT/
RUN rm -fr *
RUN echo '<h1>Hello, Docker!</h1>' > /usr/local/tomcat/webapps/ROOT/index.html
➜  tomcat sudo docker build -t mydocker .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM tomcat
 ---> 96c4e536d0eb
Step 2/4 : WORKDIR /usr/local/tomcat/webapps/ROOT/
 ---> Running in 57f471bc028d
Removing intermediate container 57f471bc028d
 ---> 9350deb03d0d
Step 3/4 : RUN rm -fr *
 ---> Running in dbaf4d7ad1d1
Removing intermediate container dbaf4d7ad1d1
 ---> f6814fab3fe4
Step 4/4 : RUN echo '<h1>Hello, Docker!</h1>' > /usr/local/tomcat/webapps/ROOT/index.html
 ---> Running in 91aa52622c26
Removing intermediate container 91aa52622c26
 ---> 4691fd38efff
Successfully built 4691fd38efff
Successfully tagged mydocker:latest
➜  tomcat sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mydocker            latest              4691fd38efff        34 seconds ago      506MB
<none>              <none>              137fe42507bf        6 minutes ago       506MB
1.16.1              latest              97423576f288        3 weeks ago         126MB
tomcat              latest              96c4e536d0eb        4 weeks ago         506MB
nginx               1.16.1              41e97992fcd8        5 weeks ago         126MB
ubuntu              16.04               5e13f8dd4c1a        8 weeks ago         120MB
➜  tomcat sudo docker run -it 4691 bash
root@33d9191d7b3b:/usr/local/tomcat/webapps/ROOT# ls
index.html
root@33d9191d7b3b:/usr/local/tomcat/webapps/ROOT# cat index.html 
<h1>Hello, Docker!</h1>
~~~
** notice 各个相同源镜像的空间是公用的 **

`docker image prune` 删除虚悬镜像

~~~
➜  tomcat sudo docker run -p 8081:8080 4691
~~~
查看网页是否成功
![](Docker学习6-Dockerfile定制镜像2/mydocker.png)

docker的又一大特性出现了,与运行环境无关,真正做到了一次构建,到处运行.

