---
title: Docker学习14-Docker-Compose-的使用
catalog: true
tags: [Docker, Docker Compose]
date: 2019-09-29 09:32:51
subtitle:
header-img:
---
# Docker-Compose-的使用

## 创建Docker-Compose.yml文件
~~~
version: '3'
services:
  tomcat:
    restart: always
    image: tomcat
    container_name: tomcat
    ports:
      - 8086:8080
~~~
## 运行docker-compose
`docker-compose up`
运行日志

~~~
[root@MyCentOS]/usr/local/docker/tomcat# docker-compose up
Starting tomcat ... done
Attaching to tomcat
tomcat    | 29-Sep-2019 03:08:29.966 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version:        Apache Tomcat/8.5.45
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built:          Aug 14 2019 22:21:25 UTC
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server number:         8.5.45.0
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name:               Linux
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Version:            3.10.0-957.27.2.el7.x86_64
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Architecture:          amd64
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Java Home:             /usr/local/openjdk-8/jre
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Version:           1.8.0_222-b10
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Vendor:            Oracle Corporation
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE:         /usr/local/tomcat
tomcat    | 29-Sep-2019 03:08:29.968 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_HOME:         /usr/local/tomcat
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djdk.tls.ephemeralDHKeySize=2048
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dorg.apache.catalina.security.SecurityListener.UMASK=0027
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dignore.endorsed.dirs=
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.base=/usr/local/tomcat
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.home=/usr/local/tomcat
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.io.tmpdir=/usr/local/tomcat/temp
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent Loaded APR based Apache Tomcat Native library [1.2.23] using APR version [1.5.2].
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR capabilities: IPv6 [true], sendfile [true], accept filters [false], random [true].
tomcat    | 29-Sep-2019 03:08:29.969 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR/OpenSSL configuration: useAprConnector [false], useOpenSSL [true]
tomcat    | 29-Sep-2019 03:08:29.993 INFO [main] org.apache.catalina.core.AprLifecycleListener.initializeSSL OpenSSL successfully initialized [OpenSSL 1.1.0k  28 May 2019]
tomcat    | 29-Sep-2019 03:08:30.155 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["http-nio-8080"]
tomcat    | 29-Sep-2019 03:08:30.175 INFO [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
tomcat    | 29-Sep-2019 03:08:30.202 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["ajp-nio-8009"]
tomcat    | 29-Sep-2019 03:08:30.203 INFO [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
tomcat    | 29-Sep-2019 03:08:30.212 INFO [main] org.apache.catalina.startup.Catalina.load Initialization processed in 984 ms
tomcat    | 29-Sep-2019 03:08:30.265 INFO [main] org.apache.catalina.core.StandardService.startInternal Starting service [Catalina]
tomcat    | 29-Sep-2019 03:08:30.265 INFO [main] org.apache.catalina.core.StandardEngine.startInternal Starting Servlet Engine: Apache Tomcat/8.5.45
tomcat    | 29-Sep-2019 03:08:30.291 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/usr/local/tomcat/webapps/docs]
tomcat    | 29-Sep-2019 03:15:19.551 WARNING [localhost-startStop-1] org.apacneratorBase.createSecureRandom Creation of SecureRandom instance for session G] took [408,728] milliseconds.
tomcat    | 29-Sep-2019 03:15:19.598 INFO [localhost-startStop-1] org.apache.deployDirectory Deployment of web application directory [/usr/local/tomcat/we[409,307] ms
tomcat    | 29-Sep-2019 03:15:19.599 INFO [localhost-startStop-1] org.apache.deployDirectory Deploying web application directory [/usr/local/tomcat/webapp
tomcat    | 29-Sep-2019 03:15:19.668 INFO [localhost-startStop-1] org.apache.deployDirectory Deployment of web application directory [/usr/local/tomcat/wein [69] ms
tomcat    | 29-Sep-2019 03:15:19.668 INFO [localhost-startStop-1] org.apache.deployDirectory Deploying web application directory [/usr/local/tomcat/webapp
tomcat    | 29-Sep-2019 03:15:19.716 INFO [localhost-startStop-1] org.apache.deployDirectory Deployment of web application directory [/usr/local/tomcat/weshed in [48] ms
tomcat    | 29-Sep-2019 03:15:19.716 INFO [localhost-startStop-1] org.apache.deployDirectory Deploying web application directory [/usr/local/tomcat/webapp
tomcat    | 29-Sep-2019 03:15:19.768 INFO [localhost-startStop-1] org.apache.deployDirectory Deployment of web application directory [/usr/local/tomcat/we[52] ms
tomcat    | 29-Sep-2019 03:15:19.768 INFO [localhost-startStop-1] org.apache.deployDirectory Deploying web application directory [/usr/local/tomcat/webapp
tomcat    | 29-Sep-2019 03:15:20.228 INFO [localhost-startStop-1] org.apache.deployDirectory Deployment of web application directory [/usr/local/tomcat/we in [460] ms
tomcat    | 29-Sep-2019 03:15:20.239 INFO [main] org.apache.coyote.AbstractProlHandler ["http-nio-8080"]
tomcat    | 29-Sep-2019 03:15:20.269 INFO [main] org.apache.coyote.AbstractProlHandler ["ajp-nio-8009"]
tomcat    | 29-Sep-2019 03:15:20.272 INFO [main] org.apache.catalina.startup.p in 410059 ms
~~~
## 查看运行中的容器
~~~
➜  tomcat sudo docker ps             
[sudo] password for ziming: 
Sorry, try again.
[sudo] password for ziming: 
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
13f2aa1609bc        tomcat              "catalina.sh run"        32 minutes ago      Up 28 minutes       0.0.0.0:8086->8080/tcp   tomcat
c8b87b5f1fcd        tomcat              "catalina.sh run"        3 days ago          Up 37 minutes       0.0.0.0:8083->8080/tcp   tripweb
1e07f46eb80a        tomcat              "catalina.sh run"        4 days ago          Up 37 minutes       0.0.0.0:8085->8080/tcp   myshop
00e3d56be365        mysql:5.7.22        "docker-entrypoint.s…"   5 days ago          Up 37 minutes       0.0.0.0:3307->3306/tcp   mysql
d53272c36764        tomcat              "catalina.sh run"        5 days ago          Up 37 minutes       0.0.0.0:8082->8080/tcp   mytomcat2
849e5b576998        myproject           "catalina.sh run"        6 days ago          Up 37 minutes       0.0.0.0:8081->8080/tcp   elastic_panini
~~~
## 浏览器访问
![](Docker学习14-Docker-Compose-的使用/1.png)
    
## 参考资料
> https://www.funtl.com/zh/docker-compose/Docker-Compose-%E4%BD%BF%E7%94%A8.html#%E5%9C%BA%E6%99%AF
