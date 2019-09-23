---
title: Docker学习06-Dockerfile定制镜像03
catalog: true
date: 2019-09-20 10:39:10
subtitle: Dockerfile定制镜像03
header-img:
tags: [Docker]
---
# Dockerfile定制镜像03

## 镜像构建上下文(Context)
`docker build -t <name> .`
这里的点表示当前目录

如果注意，会看到 docker build 命令最后有一个 .。. 表示当前目录，而 Dockerfile 就在当前目录，因此不少初学者以为这个路径是在指定 Dockerfile 所在路径，这么理解其实是不准确的。如果对应上面的命令格式，你可能会发现，这是在指定上下文路径。那么什么是上下文呢？

首先我们要理解 docker build 的工作原理。Docker 在运行时分为 Docker 引擎（也就是服务端守护进程）和客户端工具。Docker 的引擎提供了一组 REST API，被称为 Docker Remote API，而如 docker 命令这样的客户端工具，则是通过这组 API 与 Docker 引擎交互，从而完成各种功能。因此，虽然表面上我们好像是在本机执行各种 docker 功能，但实际上，一切都是使用的远程调用形式在服务端（Docker 引擎）完成。也因为这种 C/S 设计，让我们操作远程服务器的 Docker 引擎变得轻而易举。

- 拓扑图如下
~~~
host os          请求Rest Api          
docker run       <---------->      docker server
docker images        返回

host os                      请求Rest Api 带着当前目录的压缩包           docker server
docker build -t mydocker .       <--------------------->                 接收 docker build
将当前目录打包                             返回                          构建镜像

~~~
如果在 Dockerfile 中这么写：
~~~ Dockerfile
COPY ./package.json /app/
~~~

这并不是要复制执行 docker build 命令所在的目录下的 package.json，也不是复制 Dockerfile 所在目录下的 package.json，而是复制 上下文（context） 目录下的 package.json。
- 上下文环境目录
宿主机将. 当前目录打包后发送至docker server后,由docker解压打包文件后的目录.

因此，COPY 这类指令中的源文件的路径都是相对路径。这也是初学者经常会问的为什么 COPY ../package.json /app 或者 COPY /opt/xxxx /app 无法工作的原因，因为这些路径已经超出了上下文的范围，Docker 引擎无法获得这些位置的文件。如果真的需要那些文件，应该将它们复制到上下文目录中去。

现在就可以理解刚才的命令 docker build -t nginx:v3 . 中的这个 .，实际上是在指定上下文的目录，docker build 命令会将该目录下的内容打包交给 Docker 引擎以帮助构建镜像。

实际上,dockerfile在打包过程中也说明了镜像构建的上下文

## 在tomcat下新建一个index.html文件
~~~ html
<h1>Hello Docker</h1>
~~~

## 重新写dockerfile脚本
~~~ dockerfile
# Dockerfile脚本开始
FROM tomcat
WORKDIR /usr/local/tomcat/webapps/ROOT/
RUN rm -fr *
COPY index.html /usr/local/tomcat/webapps/ROOT
tomcat sudo docker build -t test .
Sending build context to Docker daemon  15.87kB
这里的context就已经构建到docker的守护进程中
Step 1/4 : FROM tomcat
 ---> 96c4e536d0eb
Step 2/4 : WORKDIR /usr/local/tomcat/webapps/ROOT/
 ---> Using cache
 ---> 9350deb03d0d
Step 3/4 : RUN rm -fr *
 ---> Using cache
 ---> f6814fab3fe4
Step 4/4 : COPY index.html /usr/local/tomcat/webapps/ROOT
 ---> 1dbbbda7e1cb
Successfully built 1dbbbda7e1cb
Successfully tagged test:latest
➜  tomcat sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
test                latest              1dbbbda7e1cb        13 seconds ago      506MB
mydocker            latest              4691fd38efff        2 hours ago         506MB
1.16.1              latest              97423576f288        3 weeks ago         126MB
tomcat              latest              96c4e536d0eb        4 weeks ago         506MB
nginx               1.16.1              41e97992fcd8        5 weeks ago         126MB
ubuntu              16.04               5e13f8dd4c1a        8 weeks ago         120MB
~~~







