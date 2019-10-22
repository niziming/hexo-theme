---
title: Nexus学习01-Nexus简介与安装
catalog: true
tags: []
date: 2019-10-22 15:19:32
subtitle:
header-img:
---
# Nexus学习01-Nexus简介与安装
Nexus 是一个强大的仓库管理器，极大地简化了内部仓库的维护和外部仓库的访问。
2016 年 4 月 6 日 Nexus 3.0 版本发布，相较 2.x 版本有了很大的改变：

- 对低层代码进行了大规模重构，提升性能，增加可扩展性以及改善用户体验。
- 升级界面，极大的简化了用户界面的操作和管理。
- 提供新的安装包，让部署更加简单。
- 增加对 Docker, NeGet, npm, Bower 的支持。
- 提供新的管理接口，以及增强对自动任务的管理。

## docker 安装nexus3
~~~ 
[root@MyCentOS]/usr/local/docker/gitlab# docker pull sonatype/nexus3
Using default tag: latest
latest: Pulling from sonatype/nexus3
c65691897a4d: Downloading [===============>                                   ]     22MB/71.03MB
641d7cc5cbc4: Download complete 
c508b13320cd: Download complete 
79e3bf9d3132: Downloading [=======>                                           ]  31.63MB/212.2MB
~~~
查看latest版本号
~~~
[root@MyCentOS]/usr/local/docker/nexus# docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
sonatype/nexus3          latest              8eb898be2a53        12 days ago         611MB
myproject                latest              98cdee09487c        4 weeks ago         506MB
mydocker                 latest              4691fd38efff        4 weeks ago         506MB
mysql                    latest              b8fd9553f1f0        5 weeks ago         445MB
1.16.1                   latest              97423576f288        7 weeks ago         126MB
tomcat                   latest              96c4e536d0eb        2 months ago        506MB
centos                   latest              67fa590cfc1c        2 months ago        202MB
nginx                    1.16.1              41e97992fcd8        2 months ago        126MB
ubuntu                   16.04               5e13f8dd4c1a        3 months ago        120MB
mysql                    5.7.22              6bb891430fb6        15 months ago       372MB
twang2218/gitlab-ce-zh   10.5                90543a7ab79b        15 months ago       1.58GB
~~~
## docker-compose.yml
~~~
version: '3.1'
services:
  nexus:
    restart: always
    image: sonatype/nexus3
    container_name: nexus
    ports:
      - 8082:8081
    volumes:
      - /usr/local/docker/nexus/data:/nexus-data
~                                                
~~~

### 挂在数据权限修改
~~~
[root@MyCentOS]/usr/local/docker/nexus# mkdir data
[root@MyCentOS]/usr/local/docker/nexus# ls
data  docker-compose.yml
[root@MyCentOS]/usr/local/docker/nexus# chmod 777 data 
~~~

## 启动Nexus
~~~
[root@MyCentOS]/usr/local/docker/nexus# docker-compose up -d
Creating network "nexus_default" with the default driver
Creating nexus ... done
[root@MyCentOS]/usr/local/docker/nexus# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
5e897f337f34        sonatype/nexus3     "sh -c ${SONATYPE_DI…"   30 seconds ago      Up 2 seconds        0.0.0.0:8082->8081/tcp   nexus
~~~
## 查看结果
![](1.png)

## 登陆Nexus
查看admin密码
~~~
[root@MyCentOS]/usr/local/docker/nexus# docker exec -it 09b bash
bash-4.4$ cd /nexus-data/
bash-4.4$ ls
admin.password	db	       generated-bundles  karaf.pid  log     restore-from-backup
blobs		elasticsearch  instances	  keystores  orient  tmp
cache		etc	       javaprefs	  lock	     port
bash-4.4$ cat admin.password 
75c4c214-cdd2-48d0-845c-03c28da34abfbash-4.4$ #  
~~~


## 参考资料
> 
