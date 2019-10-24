---
title: Nexus学习02-在项目中使用maven私库
catalog: true
tags: []
date: 2019-10-23 10:55:40
subtitle:
header-img:
---
# Nexus学习02-在项目中使用maven私库

## 配置Maven的setting.xml文件

### Snapshots 与 Releases 的区别
- nexus-releases: 用于发布 Release 版本
- nexus-snapshots: 用于发布 Snapshot 版本（快照版）
Release 版本与 Snapshot 定义如下
~~~ xml
<servers>
    <!-- server
     | Specifies the authentication information to use when connecting to a particular server, identified by
     | a unique name within the system (referred to by the 'id' attribute below).
     |
     | NOTE: You should either specify username/password OR privateKey/passphrase, since these pairings are
     |       used together.
     |
    <server>
      <id>deploymentRepo</id>
      <username>repouser</username>
      <password>repopwd</password>
    </server>
    -->
	<!-- 发行版本私服仓库 -->
	<server>
      <id>maven-releases</id>
      <username>admin</username>
      <password>admin</password>
    </server>
	 <!-- 快照版本私服仓库 -->
	<server>
      <id>maven-snapshots</id>
      <username>admin</username>
      <password>admin</password>
    </server>

    <!-- Another sample, using keys to authenticate.
    <server>
      <id>siteServer</id>
      <privateKey>/path/to/private/key</privateKey>
      <passphrase>optional; leave empty if not used.</passphrase>
    </server>
    -->
  </servers>
~~~
## 配置自动化部署
`pom.xml`中添加代码如下

~~~
<distributionManagement>  
  <repository>  
    <id>nexus-releases</id>  
    <name>Nexus Release Repository</name>  
    <url>http://127.0.0.1:8081/repository/maven-releases/</url>  
  </repository>  
  <snapshotRepository>  
    <id>nexus-snapshots</id>  
    <name>Nexus Snapshot Repository</name>  
    <url>http://127.0.0.1:8081/repository/maven-snapshots/</url>  
  </snapshotRepository>  
</distributionManagement> 
~~~
 
![](1.png)

### 修改私服地址
在nexus中查看地址
![](2.png)

## 使用maven打包推送
`mvn deploy`
`mvn deploy -Dmaven.test.skip=true` 去掉测试类
~~~
ziming@Dell17r2 MINGW64 /d/IdeaProjects/tripweb (master)
$ mvn deploy -Dmaven.test.skip=true
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building trip-web Maven Webapp 1.0.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- mybatis-generator-maven-plugin:1.3.5:generate (Generate MyBatis Artifacts) @ trip-web ---
[INFO] Connecting to the Database
Wed Oct 23 11:20:50 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ an
d 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the ve
rifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore f
or server certificate verification.

~~~
### 出现错误 Not authorized , ReasonPhrase:Unauthorized
~~~
[INFO] Final Memory: 28M/378M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.8.2:deploy (default-deploy) on project trip-web: Failed to retrieve remote metadat
a cn.zm:trip-web:1.0.0-SNAPSHOT/maven-metadata.xml: Could not transfer metadata cn.zm:trip-web:1.0.0-SNAPSHOT/maven-metadata.xml from/to nexus-snapshots (http:/
/116.62.110.99:8082/repository/maven-snapshots/): Not authorized , ReasonPhrase:Unauthorized. -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
~~~

![](3.png)
将的 Deployment Policy 修改为 “Allow Redeploy”

![](4.png)
### 修改pom.xml配置
~~~
<!-- 发行版本私服仓库 -->
	<server>
      <id>maven-releases</id>
      <username>admin</username>
      <password>admin</password>
	  <!--鉴权时使用的私钥密码。-->
	  <passphrase>admin</passphrase>
	  <!--文件被创建时的权限。如果在部署的时候会创建一个仓库文件或者目录，这时候就可以使用权限（permission）。这两个元素合法的值是一个三位数字，其对应了unix文件系统的权限，如664，或者775。 -->
	  <filePermissions>664</filePermissions>
      <!--目录被创建时的权限。 -->
      <directoryPermissions>775</directoryPermissions>
   	</server>
	<!-- 快照版本私服仓库 -->
	<server>
      <id>maven-snapshots</id>
      <username>admin</username>
      <password>admin</password>
	  <!--鉴权时使用的私钥密码。-->
	  <passphrase>admin</passphrase>
	  <!--文件被创建时的权限。如果在部署的时候会创建一个仓库文件或者目录，这时候就可以使用权限（permission）。这两个元素合法的值是一个三位数字，其对应了unix文件系统的权限，如664，或者775。 -->
	  <filePermissions>664</filePermissions>
      <!--目录被创建时的权限。 -->
      <directoryPermissions>775</directoryPermissions>
    </server>
~~~

## 参考资料
> 
