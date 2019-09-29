---
title: Docker学习17-Docker-Compose-部署项目到容器
catalog: true
tags: []
date: 2019-09-29 14:31:35
subtitle:
header-img:
---

# Docker学习17-Docker-Compose-部署项目到容器

## 删除原先所有容器
~~~
[root@MyCentOS]/usr/local/docker# docker rm $(docker ps -a) 
tripweb
1e07f46eb80a
00e3d56be365
d53272c36764
tomcat
849e5b576998
~~~

## 创建项目tripweb



version: '3'
services:
  tomcat:
    restart: always
    image: tomcat
    container_name: tomcat
    ports:
      - 8086:8080
  mysql:
    restart: always
    image: mysql
    container_name: mysql    
    ports:
      - 3308:3306

##


## 参考资料
> 
