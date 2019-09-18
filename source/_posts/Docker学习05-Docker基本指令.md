---
title: Docker学习05-Docker基本指令
catalog: true
date: 2019-09-18 16:05:05
subtitle:
header-img:
tags: [Docker]
---

# Docker基本指令
从 Docker 镜像仓库获取镜像的命令是 docker pull
- docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
列出镜像
- docker image ls
虚悬镜像
- docker image prune
中间层镜像
- docker image ls -a
列出部分镜像
- docker image ls ubuntu
- docker image ls ubuntu:16.04
docker image ls 还支持强大的过滤器参数 --filter
希望看到在 mongo:3.2 之后建立的镜像
- docker image ls -f since=mongo:3.2
想查看某个位置之前的镜像也可以，只需要把 since 换成 before 即可。
此外，如果镜像构建时，定义了 LABEL，还可以通过 LABEL 来过滤。

## 以特定格式显示
利用 docker image ls 把所有的虚悬镜像的 ID 列出来，然后才可以交给 docker image rm 命令作为参数来删除指定的这些镜像，这个时候就用到了 -q 参数。
- docker image ls -q
下面的命令会直接列出镜像结果，并且只包含镜像 ID 和仓库名：打算以表格等距显示，并且有标题行，和默认一样，不过自己定义列：
- docker image ls --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"

# 删除本地镜像
删除本地的镜像
- docker image rm [选项] <镜像1> [<镜像2> ...]
~~~ log
$ docker image ls
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
centos                      latest              0584b3d2cf6d        3 weeks ago         196.5 MB
redis                       alpine              501ad78535f0        3 weeks ago         21.03 MB
docker                      latest              cf693ec9b5c7        3 weeks ago         105.1 MB
nginx                       latest              e43d811ce2f4        5 weeks ago         181.5 MB
~~~
~~~ log
$ docker image rm 501
Untagged: redis:alpine
Untagged: redis@sha256:f1ed3708f538b537eb9c2a7dd50dc90a706f7debd7e1196c9264edeea521a86d
Deleted: sha256:501ad78535f015d88872e13fa87a828425117e3d28075d0c117932b05bf189b7
Deleted: sha256:96167737e29ca8e9d74982ef2a0dda76ed7b430da55e321c071f0dbff8c2899b
Deleted: sha256:32770d1dcf835f192cafd6b9263b7b597a1778a403a109e2cc2ee866f74adf23
Deleted: sha256:127227698ad74a5846ff5153475e03439d96d4b1c7f2a449c7a826ef74a2d2fa
Deleted: sha256:1333ecc582459bac54e1437335c0816bc17634e131ea0cc48daa27d32c75eab3
Deleted: sha256:4fc455b921edf9c4aea207c51ab39b10b06540c8b4825ba57b3feed1668fa7c7
~~~ 
用镜像名，也就是 <仓库名>:<标签>，来删除镜像。
~~~
$ docker image rm centos
Untagged: centos:latest
Untagged: centos@sha256:b2f9d1c0ff5f87a4743104d099a3d561002ac500db1b9bfa02a783a46e0d366c
Deleted: sha256:0584b3d2cf6d235ee310cf14b54667d889887b838d3f3d3033acd70fc3c48b8a
Deleted: sha256:97ca462ad9eeae25941546209454496e1d66749d53dfa2ee32bf1faabd239d38
~~~
使用 镜像摘要 删除镜像

~~~
$ docker image ls --digests
REPOSITORY                  TAG                 DIGEST                                                                    IMAGE ID            CREATED             SIZE
node                        slim                sha256:b4f0e0bdeb578043c1ea6862f0d40cc4afe32a4a582f3be235a3b164422be228   6e0c4c8e3913        3 weeks ago         214 MB

$ docker image rm node@sha256:b4f0e0bdeb578043c1ea6862f0d40cc4afe32a4a582f3be235a3b164422be228
Untagged: node@sha256:b4f0e0bdeb578043c1ea6862f0d40cc4afe32a4a582f3be235a3b164422be228
~~~
## Untagged 和 Deleted
Untagged，另一类是 Deleted

## 用 docker image ls 命令来配合
像其它可以承接多个实体的命令一样，可以使用 docker image ls -q 来配合使用 docker image rm，这样可以成批的删除希望删除的镜像。我们在 “镜像列表” 章节介绍过很多过滤镜像列表的方式都可以拿过来使用。

比如，我们需要删除所有仓库名为 redis 的镜像：
~~~
$ docker image rm $(docker image ls -q redis)
~~~
或者删除所有在 mongo:3.2 之前的镜像：
~~~
$ docker image rm $(docker image ls -q -f before=mongo:3.2)
~~~


