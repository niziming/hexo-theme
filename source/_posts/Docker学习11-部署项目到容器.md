---
title: Docker学习11-部署项目到容器
catalog: true
date: 2019-09-25 11:06:25
subtitle:
header-img:
tags: [Docker]
---
# 部署项目到容器

## 打包项目的war包
这里用的是我毕业设计的项目,基于SSM的旅游项目网站.
`mvn package'
![](1.png)

## 在本地测试项目是否可以正常启动
- 将项目解压到ROOT中
![](2.png)

- 启动tomcat
![](3.png)

- 浏览器端查看
![](4.png)

- 查看控制台
![](5.png)

## server端操作

### 上传war到ECS中
![](6.png)

### 将war移动到ROOT中做数据卷挂载
~~~
[root@MyCentOS]~# cd /root
[root@MyCentOS]~# ls
authorized_keys
mysql57-community-release-el7-10.noarch.rpm
mysql57-community-release-el7-11.noarch.rpm
mysql-community-release-el7-5.noarch.rpm
mysql-community-release-el7-5.noarch.rpm.1
mysql-community-release-el7-5.noarch.rpm.2
trip-web.war
[root@MyCentOS]~# mv trip-web.war /usr/local/docker/tomcat/ROOT
[root@MyCentOS]~# cd /usr/local/docker/tomcat/ROOT
[root@MyCentOS]/usr/local/docker/tomcat/ROOT# ll
total 45080
-rw-r--r-- 1 root root      172 Sep 23 16:57 index.html
-rw-r--r-- 1 root root 46155747 Sep 25 14:44 trip-web.war
[root@MyCentOS]/usr/local/docker/tomcat/ROOT# rm index.html
[root@MyCentOS]/usr/local/docker/tomcat/ROOT# tar
~~~

### 解压并修改数据库端口
- 解压
~~~
[root@MyCentOS]/usr/local/docker/tomcat/ROOT# unzip -oq trip-web.war
[root@MyCentOS]/usr/local/docker/tomcat/ROOT# ls
META-INF  static  trip-web.war  WEB-INF
~~~
- 修改
~~~
[root@MyCentOS]/usr/local/docker/tomcat/ROOT/WEB-INF/classes# ls
cn                   mybatis-config.xml          spring-context.xml
generatorConfig.xml  rebel.xml                   spring-mvc.xml
log4j.properties     spring-context-druid.xml    trip_web_db.sql
mapper               spring-context-mybatis.xml  tripweb.properties
[root@MyCentOS]/usr/local/docker/tomcat/ROOT/WEB-INF/classes# vim tripweb.properties
~~~
![](7.png)

### 移走war包
~~~
[root@MyCentOS]/usr/local/docker/tomcat/ROOT/WEB-INF/classes# cd ../..
[root@MyCentOS]/usr/local/docker/tomcat/ROOT# ls
META-INF  static  trip-web.war  WEB-INF
[root@MyCentOS]/usr/local/docker/tomcat/ROOT# mv trip-web.war /root
[root@MyCentOS]/usr/local/docker/tomcat/ROOT# ls
META-INF  static  WEB-INF
[root@MyCentOS]/usr/local/docker/tomcat/ROOT#
~~~
## docker数据库

![](8.png)


## 修改项目数据库ip及端口和用户
~~~
# JDBC
# MySQL 8.x: com.mysql.cj.jdbc.Driver
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://116.62.110.99:3307/trip_web_db?useUnicode=true&characterEncoding=UTF-8
jdbc.username=root
jdbc.password=root
~~~

### 挂载数据卷运行
- 挂载数据卷运行
`docker run -p 8080:8080 --name myshop -v /usr/local/docker/tomcat/ROOT/:/usr/local/tomcat/webapps/ROOT -d tomcat`
~~~
[root@MyCentOS]/usr/local/docker/tomcat/ROOT# docker run -p 8086:8080 --name tripweb -v /usr/local/docker/tomcat/ROOT/:/usr/local/tomcat/webapps/ROOT -d tomcat
4df4ef7a219385ebbb818f65f238bfdd0d54f1d65249e2afd653394ccdffb901
~~~

`docker logs myshop`
查看docker容器的日志
~~~
➜  ROOT sudo docker logs myshop
25-Sep-2019 03:11:48.843 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version:        Apache Tomcat/8.5.45
25-Sep-2019 03:11:48.902 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built:          Aug 14 2019 22:21:25 UTC
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server number:         8.5.45.0
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name:               Linux
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Version:            3.10.0-957.27.2.el7.x86_64
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Architecture:          amd64
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Java Home:             /usr/local/openjdk-8/jre
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Version:           1.8.0_222-b10
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Vendor:            Oracle Corporation
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE:         /usr/local/tomcat
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_HOME:         /usr/local/tomcat
25-Sep-2019 03:11:48.903 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djdk.tls.ephemeralDHKeySize=2048
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dorg.apache.catalina.security.SecurityListener.UMASK=0027
25-# JDBC
# MySQL 8.x: com.mysql.cj.jdbc.Driver
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://116.62.110.99:3307/trip_web_db?useUnicode=true&characterEncoding=UTF-8
jdbc.username=root
jdbc.password=rootSep-2019 03:11:48.904 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dignore.endorsed.dirs=
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.base=/usr/local/tomcat
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.home=/usr/local/tomcat
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.io.tmpdir=/usr/local/tomcat/temp
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent Loaded APR based Apache Tomcat Native library [1.2.23] using APR version [1.5.2].
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR capabilities: IPv6 [true], sendfile [true], accept filters [false], random [true].
25-Sep-2019 03:11:48.904 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR/OpenSSL configuration: useAprConnector [false], useOpenSSL [true]
25-Sep-2019 03:11:48.922 INFO [main] org.apache.catalina.core.AprLifecycleListener.initializeSSL OpenSSL successfully initialized [OpenSSL 1.1.0k  28 May 2019]
25-Sep-2019 03:11:49.150 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["http-nio-8080"]
25-Sep-2019 03:11:49.182 INFO [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
25-Sep-2019 03:11:49.228 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["ajp-nio-8009"]
25-Sep-2019 03:11:49.230 INFO [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
25-Sep-2019 03:11:49.237 INFO [main] org.apache.catalina.startup.Catalina.load Initialization processed in 1809 ms
25-Sep-2019 03:11:49.299 INFO [main] org.apache.catalina.core.StandardService.startInternal Starting service [Catalina]
25-Sep-2019 03:11:49.299 INFO [main] org.apache.catalina.core.StandardEngine.startInternal Starting Servlet Engine: Apache Tomcat/8.5.45
25-Sep-2019 03:11:49.333 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/usr/local/tomcat/webapps/docs]
25-Sep-2019 03:12:14.400 WARNING [localhost-startStop-1] org.apache.catalina.util.SessionIdGeneratorBase.createSecureRandom Creation of SecureRandom instance for session ID generation using [SHA1PRNG] took [24,548] milliseconds.
25-Sep-2019 03:12:14.531 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/usr/local/tomcat/webapps/docs] has finished in [25,197] ms
25-Sep-2019 03:12:14.531 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/usr/local/tomcat/webapps/manager]
25-Sep-2019 03:12:14.597 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/usr/local/tomcat/webapps/manager] has finished in [66] ms
25-Sep-2019 03:12:14.598 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/usr/local/tomcat/webapps/host-manager]
25-Sep-2019 03:12:14.637 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/usr/local/tomcat/webapps/host-manager] has finished in [39] ms
25-Sep-2019 03:12:14.637 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/usr/local/tomcat/webapps/ROOT]
25-Sep-2019 03:12:14.756 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/usr/local/tomcat/webapps/ROOT] has finished in [119] ms
25-Sep-2019 03:12:14.756 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/usr/local/tomcat/webapps/examples]
25-Sep-2019 03:12:15.189 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/usr/local/tomcat/webapps/examples] has finished in [433] ms
25-Sep-2019 03:12:15.218 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]
25-Sep-2019 03:12:15.267 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["ajp-nio-8009"]
25-Sep-2019 03:12:15.270 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in 26033 ms
~~~

docker logs -f myshop
会一直监听日志的变化

## 查看项目

![](9.png)

## 参考资料
> https://www.funtl.com/zh/guide/%E8%B5%B0%E5%90%91%E5%8D%95%E4%BD%93%E5%9C%B0%E7%8B%B1.html#%E8%A7%86%E9%A2%91%E5%90%88%E9%9B%86




