---
title: Docker学习9-Docker数据卷
catalog: true
date: 2019-09-23 15:54:04
subtitle:
header-img:
tags: [Docker]
---
# Docker数据卷
数据卷 是一个可供一个或多个容器使用的特殊目录，它绕过 UFS，可以提供很多有用的特性：

- 数据卷 可以在容器之间共享和重用
- 对 数据卷 的修改会立马生效
- 对 数据卷 的更新，不会影响镜像
- 数据卷 默认会一直存在，即使容器被删除
> 注意：数据卷 的使用，类似于 Linux 下对目录或文件进行 mount，镜像中的被指定为挂载点的目录中的文件会隐藏掉，能显示看的是挂载的 数据卷。

![](Docker学习9-Docker数据卷/1.png)

数据卷 Docker容器的数据持久化

## 使用数据卷
~~~
➜  ROOT sudo docker run -p 8083:8080 --name volumeDemo -d -v /usr/local/docker/tomcat/ROOT:/usr/local/tomcat/webapps/ROOT tomcat
[sudo] password for ziming: 
52ae0e61dcf59e6f02f3a05c2173fd5c7d8966e99c0bda725573cae58b393fed
➜  ROOT sudo docker run -p 8083:8080 --name volumeDemo -d -v /usr/local/docker/tomcat/ROOT:/usr/local/tomcat/webapps/ROOT tomcat
[sudo] password for ziming:
52ae0e61dcf59e6f02f3a05c2173fd5c7d8966e99c0bda725573cae58b393fed
~~~

`docker exec -it tomcat bash`交互的方式进入
~~~
tomcat sudo docker run -p 8083:8080 --name volumeDemo -d -v /us
r/local/docker/tomcat/ROOT/:/usr/local/tomcat/webapps/ROOT tomcat
a569649fcc5abaf4dbf4b6be4bd031ce57cafe27e7082fd687f90e3ab2244a78
➜  tomcat sudo docker exec -it volumeDemo bash
root@a569649fcc5a:/usr/local/tomcat# cd webapps/ROOT/
root@a569649fcc5a:/usr/local/tomcat/webapps/ROOT# ls
index.html
root@a569649fcc5a:/usr/local/tomcat/webapps/ROOT# cat index.html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Docker文件测试</title>
</head>

<body>
	<h1>Hello Docker</h1>
	<h2>数据卷测试页</h2>
</body>

</html>
root@a569649fcc5a:/usr/local/tomcat/webapps/ROOT#
~~~

## 查看结果
- volumeDemo
![](Docker学习9-Docker数据卷/2.png)

- volumeDemo1
![](Docker学习9-Docker数据卷/3.png)

## 参考资料
> https://www.funtl.com/zh/docker/Docker-%E6%95%B0%E6%8D%AE%E5%8D%B7.html#%E9%80%89%E6%8B%A9-v-%E8%BF%98%E6%98%AF-%E2%80%93mount-%E5%8F%82%E6%95%B0








