---
title: Docker学习2-Docker镜像加速器
catalog: true
tags: [Docker]
date: 2019-09-18 08:56:38
subtitle:
header-img:
---
# Docker学习2-Docker镜像加速器

## Docker学习01-Docker镜像加速器
对于使用 systemd 的系统，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）
~~~
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
 ]
}
~~~
注意，一定要保证该文件符合 json 规范，否则 Docker 将不能启动。
之后重新启动服务。

$ sudo systemctl daemon-reload
$ sudo systemctl restart docker

命令行执行`docker info`
查看内容

~~~
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
  https://registry.docker-cn.com/
 Live Restore Enabled: false
~~~

## 参考资料
> 
