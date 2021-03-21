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
~~~
cd /usr/local/docker

mkdir tripweb
~~~
## 创建docker-compose.yml
~~~
cd tripweb

vim docker-compose.yml

version: '3'
services:
  web:
    restart: always
    image: tomcat
    container_name: web
    ports:
      - 8081:8080
    volumes: # 挂载卷
      - /usr/local/docker/tripweb/ROOT:/usr/local/tomcat/webapps/ROOT
  mysql:
    restart: always
    image: mysql:5.7.22
    container_name: mysql    
    ports:
      - 3307:3306
    environment:
      TZ: Asia/Shanghai # 时区
      MYSQL_ROOT_PASSWORD: root
    command: # mysql配置
      --character-set-server=utf8
      --collation-server=utf8_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
      --max_allowed_packet=128M
      --sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO"
    volumes: # 挂载卷
      - /usr/local/docker/mysql/conf:/etc/mysql
      - /usr/local/docker/mysql/logs:/var/log/mysql
      - /usr/local/docker/mysql/data:/var/lib/mysql
~~~
上面的:
~~~
volumes: # 挂载卷
      - /usr/local/docker/mysql/conf:/etc/mysql
      - /usr/local/docker/mysql/logs:/var/log/mysql
      - /usr/local/docker/mysql/data:/var/lib/mysql
~~~
可以写成
~~~
mysql:
    volumes: # 挂载卷
        - mysql-data:/var/lib/mysql
volumes: # 挂载卷
    - mysql-data:
~~~
## 守护态运行
~~~
[root@MyCentOS]/usr/local/docker/tripweb# docker-compose up -d
Creating mysql ... done
Creating web   ... done
~~~
~~~
➜  tripweb sudo docker ps -a   
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
8a1c9fae9d1c        mysql:5.7.22        "docker-entrypoint.s…"   17 seconds ago      Up 15 seconds       0.0.0.0:3307->3306/tcp   mysql
c771ba85ec82        tomcat              "catalina.sh run"        17 seconds ago      Up 15 seconds       0.0.0.0:8081->8080/tcp   web
~~~

## 查看结果
![](Docker学习17-Docker-Compose-部署项目到容器/1.png)
![](Docker学习17-Docker-Compose-部署项目到容器/2.png)

## 部署tripweb项目到容器
~~~
[root@MyCentOS]/usr/local/docker/tripweb# cd /root 
[root@MyCentOS]~# ls
authorized_keys                              mysql-community-release-el7-5.noarch.rpm    trip-web.war
mysql57-community-release-el7-10.noarch.rpm  mysql-community-release-el7-5.noarch.rpm.1
mysql57-community-release-el7-11.noarch.rpm  mysql-community-release-el7-5.noarch.rpm.2
[root@MyCentOS]~# cp trip-web.war /usr/local/docker/tripweb
[root@MyCentOS]~# cd /usr/local/docker/tripweb
[root@MyCentOS]/usr/local/docker/tripweb# ls
docker-compose.yml  ROOT  trip-web.war
[root@MyCentOS]/usr/local/docker/tripweb# mv trip-web.war ROOT 
[root@MyCentOS]/usr/local/docker/tripweb# ls
docker-compose.yml  ROOT
[root@MyCentOS]/usr/local/docker/tripweb# cd ROOT 
[root@MyCentOS]/usr/local/docker/tripweb/ROOT# ls
trip-web.war
[root@MyCentOS]/usr/local/docker/tripweb/ROOT# unzip -oq trip-web.war -d .
[root@MyCentOS]/usr/local/docker/tripweb/ROOT# ls
META-INF  static  trip-web.war  WEB-INF
[root@MyCentOS]/usr/local/docker/tripweb/ROOT# rm trip-web.war 
[root@MyCentOS]/usr/local/docker/tripweb/ROOT# ls
META-INF  static  WEB-INF
~~~
`unzip -oq trip-web.war -d .`解压war包

## 删除正在运行的容器
~~~
➜  tripweb sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
8a1c9fae9d1c        mysql:5.7.22        "docker-entrypoint.s…"   12 minutes ago      Up 12 minutes       0.0.0.0:3307->3306/tcp   mysql
c771ba85ec82        tomcat              "catalina.sh run"        12 minutes ago      Up 12 minutes       0.0.0.0:8081->8080/tcp   web
➜  tripweb sudo docker rm -f 8a c7
8a
c7
~~~
## 运行容器
`docker-compose up -d`
守护态运行
~~~
[root@MyCentOS]/usr/local/docker/tripweb/ROOT# docker-compose up -d                       
Creating mysql ... done
Creating web   ... done
[root@MyCentOS]/usr/local/docker/tripweb/ROOT# 
~~~

## 监听日志的变化
docker logs -f myshop

## 查看结果
![](Docker学习17-Docker-Compose-部署项目到容器/3.png)

## 参考资料
> https://www.funtl.com/zh/docker-compose/Docker-Compose-模板文件.html#build
