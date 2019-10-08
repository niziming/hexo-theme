---
title: GitLab学习01-GitLab的使用
catalog: true
tags: []
date: 2019-09-30 11:06:48
subtitle:
header-img:
---
# GitLab简介
GitLab 是利用 Ruby on Rails 一个开源的版本管理系统，实现一个自托管的 Git 项目仓库，
可通过 Web 界面进行访问公开的或者私人项目。它拥有与 Github 类似的功能，能够浏览源代码，
管理缺陷和注释。可以管理团队对仓库的访问，它非常易于浏览提交过的版本并提供一个文件历史库。
团队成员可以利用内置的简单聊天程序 (Wall) 进行交流。
它还提供一个代码片段收集功能可以轻松实现代码复用，便于日后有需要的时候进行查找。
**与Github类似.**

## 为什么使用GitLab?
- 协同开发
- 异地开发
- 开源的第三方平台用
- 自己架设Git托管平台

## 安装GitLab

Docker官网汉化版GitLab
https://hub.docker.com/r/twang2218/gitlab-ce-zh

拉取GitLab: `docker pull twang2218/gitlab-ce-zh:10.5`
~~~
[root@MyCentOS]~# cd /usr/local/docker 
[root@MyCentOS]/usr/local/docker# ls
mysql  tripweb  volumes
[root@MyCentOS]/usr/local/docker# mkdir GitLab
[root@MyCentOS]/usr/local/docker# ls
GitLab  mysql  tripweb  volumes
[root@MyCentOS]/usr/local/docker# cd GitLab 
[root@MyCentOS]/usr/local/docker/GitLab# ls
[root@MyCentOS]/usr/local/docker/GitLab# vim docker-compose.yml
[root@MyCentOS]/usr/local/docker/GitLab# docker pull twang2218/gitlab-ce-zh:10.5
10.5: Pulling from twang2218/gitlab-ce-zh
d3938036b19c: Pull complete 
a9b30c108bda: Pull complete 
67de21feec18: Pull complete 
817da545be2b: Pull complete 
d967c497ce23: Pull complete 
a69c22baab05: Pull complete 
bf709c1867c3: Waiting 
89c3c3b9c89c: Waiting 
eb1d5a08657b: Waiting 
6445958fc33d: Waiting 
bd40a9292cbd: Waiting 
6e3fbbdad5bd: Waiting 
3647066e74ea: Waiting 
11d43d0755c1: Waiting 
a41004476bab: Waiting 
e3e643254137: Waiting 
b81c640d2799: Waiting 
c19aff5b9429: Waiting 
6a28e1c33c11: Waiting 
~~~

## docker-compose
~~~
mkdir gitlab
cd gitlab
vim docker-compose.yml

[root@MyCentOS]/usr/local/docker/gitlab# pwd
/usr/local/docker/gitlab
~~~
docker-compose.xml
~~~
version: '3'
services:
    gitlab:
      image: 'twang2218/gitlab-ce-zh:10.5'
      restart: always
      hostname: '116.62.110.99' # 主机名ECS的固定ip
      environment: # 环境变量
        TZ: 'Asia/Shanghai'
        GITLAB_OMNIBUS_CONFIG: | # gitlib的初始化操作
          external_url 'http://116.62.110.99' # web访问地址 可以加端口:
          gitlab_rails['time_zone'] = 'Asia/Shanghai'
          gitlab_rails['gitlab_shell_ssh_port'] = 2222 # ssh访问原本22已被占用,所以使用2222
          unicorn['port'] = 8082 # 本身的端口
          nginx['listen_port'] = 80 # 要与external的端口一致
      ports: # 批量暴露
        - '80:80'
        - '8443:443' # 443是https的端口 改为8443
        - '2222:22' # ssh的暴露端口
      volumes:
        - /usr/local/docker/gitlab/config:/etc/gitlab
        - /usr/local/docker/gitlab/data:/var/opt/gitlab
        - /usr/local/docker/gitlab/logs:/var/log/gitlab
~~~

## 启动
`docker-compose up`或者守护态运行`docker-compose up -d`
然后监听日志`docker logs -f 容器id或者name`

输入这条命令以后会抛出大量的日志,等待即可.
~~~
......
transactions"
web_1  |     - change mode from '' to '0644'
web_1  |     - change owner from '' to 'gitlab-psql'
web_1  |   * execute[/opt/gitlab/bin/gitlab-ctl start postgres-exporter] action run
web_1  |     [execute] ok: run: postgres-exporter: (pid 1149) 12s
web_1  |     - execute /opt/gitlab/bin/gitlab-ctl start postgres-exporter
web_1  | Recipe: gitlab::gitlab-rails
~~~
## 查看结果
发现无法访问,因该是ecs的端口安全组策略未开放.

![](1.png)

![](2.png)

再次启动后访问,如图所示
![](3.png)

## 解决错误

### 交互方式进入容器
`sudo docker exec -it f9 bash`

~~~
root@116:/# cd /var/log/gitlab/nginx
root@116:/var/log/gitlab/nginx# ls
access.log  config  current  error.log  gitlab_access.log  gitlab_error.log  lock
root@116:/var/log/gitlab/nginx# vi error.log 
root@116:/var/log/gitlab/nginx# ls
access.log  config  current  error.log  gitlab_access.log  gitlab_error.log  lock
root@116:/var/log/gitlab/nginx# vi gitlab_error.log 
root@116:/var/log/gitlab/nginx# gitlab-ctl status
run: gitaly: (pid 436) 3751s; run: log: (pid 430) 3751s
run: gitlab-monitor: (pid 442) 3751s; run: log: (pid 427) 3751s
run: gitlab-workhorse: (pid 448) 3752s; run: log: (pid 443) 3752s
run: logrotate: (pid 5093) 151s; run: log: (pid 432) 3752s
run: nginx: (pid 438) 3752s; run: log: (pid 433) 3752s
run: node-exporter: (pid 441) 3752s; run: log: (pid 434) 3752s
run: postgres-exporter: (pid 440) 3752s; run: log: (pid 428) 3752s
run: postgresql: (pid 452) 3752s; run: log: (pid 447) 3752s
run: prometheus: (pid 435) 3752s; run: log: (pid 429) 3752s
run: redis: (pid 451) 3752s; run: log: (pid 446) 3752s
run: redis-exporter: (pid 449) 3752s; run: log: (pid 444) 3752s
run: sidekiq: (pid 5335) 5s; run: log: (pid 431) 3752s
run: sshd: (pid 24) 3762s; run: log: (pid 23) 3762s
run: unicorn: (pid 5283) 32s; run: log: (pid 445) 3752s
root@116:/var/log/gitlab/nginx# gitlab-ctl tail unicorn
==> /var/log/gitlab/unicorn/unicorn_stdout.log <==

==> /var/log/gitlab/unicorn/unicorn_stderr.log <==
I, [2019-10-08T05:43:15.297148 #4587]  INFO -- : Refreshing Gem list
I, [2019-10-08T05:45:33.002415 #4689]  INFO -- : Refreshing Gem list
I, [2019-10-08T05:46:43.812719 #4792]  INFO -- : Refreshing Gem list
I, [2019-10-08T05:47:29.594381 #4856]  INFO -- : Refreshing Gem list
I, [2019-10-08T05:49:13.121405 #4963]  INFO -- : Refreshing Gem list
I, [2019-10-08T05:50:03.751939 #5033]  INFO -- : Refreshing Gem list
I, [2019-10-08T05:51:27.681477 #5137]  INFO -- : Refreshing Gem list
I, [2019-10-08T05:52:25.463628 #5221]  INFO -- : Refreshing Gem list
I, [2019-10-08T05:53:26.002010 #5314]  INFO -- : Refreshing Gem list
I, [2019-10-08T05:54:21.678943 #5399]  INFO -- : Refreshing Gem list

==> /var/log/gitlab/unicorn/current <==
2019-10-08_05:51:24.53737 starting new unicorn master
2019-10-08_05:52:19.58587 master failed to start, check stderr log for details
2019-10-08_05:52:21.69616 failed to start a new unicorn master
2019-10-08_05:52:21.89708 starting new unicorn master
2019-10-08_05:53:20.58943 master failed to start, check stderr log for details
2019-10-08_05:53:22.95301 failed to start a new unicorn master
2019-10-08_05:53:23.14569 starting new unicorn master
2019-10-08_05:54:17.31323 master failed to start, check stderr log for details
2019-10-08_05:54:19.23802 failed to start a new unicorn master
2019-10-08_05:54:19.35571 starting new unicorn master
2019-10-08_05:55:19.31273 master failed to start, check stderr log for details
2019-10-08_05:55:21.71564 failed to start a new unicorn master
2019-10-08_05:55:21.92552 starting new unicorn master

==> /var/log/gitlab/unicorn/unicorn_stderr.log <==
I, [2019-10-08T05:55:24.691724 #5492]  INFO -- : Refreshing Gem list

==> /var/log/gitlab/unicorn/current <==
2019-10-08_05:56:06.09911 master failed to start, check stderr log for details
2019-10-08_05:56:08.52929 failed to start a new unicorn master
2019-10-08_05:56:08.74450 starting new unicorn master

==> /var/log/gitlab/unicorn/unicorn_stderr.log <==
I, [2019-10-08T05:56:11.797988 #5571]  INFO -- : Refreshing Gem list

==> /var/log/gitlab/unicorn/current <==
2019-10-08_05:56:55.14527 master failed to start, check stderr log for details
2019-10-08_05:56:57.66708 failed to start a new unicorn master
2019-10-08_05:56:57.86318 starting new unicorn master
^C/opt/gitlab/embedded/bin/omnibus-ctl: Interrupt
~~~

提示 `failed to start a new unicorn master`

### 查看报错日志
~~~
root@116:/var/log/gitlab/unicorn# vi unicorn_stderr.log

I, [2019-10-08T04:48:12.284632 #556]  INFO -- : Refreshing Gem list
Warning: fuzzy message was ignored.
  : msgid 'You are on a read-only GitLab instance.'
I, [2019-10-08T04:49:03.098603 #696]  INFO -- : Refreshing Gem list
I, [2019-10-08T04:50:01.069664 #912]  INFO -- : Refreshing Gem list
I, [2019-10-08T04:51:26.513453 #521]  INFO -- : Refreshing Gem list
Warning: fuzzy message was ignored.
  : msgid 'You are on a read-only GitLab instance.'
I, [2019-10-08T04:52:36.836487 #693]  INFO -- : Refreshing Gem list
Warning: fuzzy message was ignored.
  : msgid 'You are on a read-only GitLab instance.'
I, [2019-10-08T04:53:40.245197 #777]  INFO -- : Refreshing Gem list
Warning: fuzzy message was ignored.
  : msgid 'You are on a read-only GitLab instance.'
I, [2019-10-08T04:54:42.016154 #867]  INFO -- : Refreshing Gem list
Warning: fuzzy message was ignored.
  : msgid 'You are on a read-only GitLab instance.'
I, [2019-10-08T04:55:41.475464 #948]  INFO -- : Refreshing Gem list
Warning: fuzzy message was ignored.
  : msgid 'You are on a read-only GitLab instance.'
I, [2019-10-08T04:56:41.565244 #1031]  INFO -- : Refreshing Gem list
Warning: fuzzy message was ignored.
~~~
提示: You are on a read-only GitLab instance.

### 授权
~~~
# chmod -R 777 /var/log/gitlab
# gitlab-ctl tail unicorn
~~~

### 重启容器
~~~
[root@MyCentOS]/usr/local/docker/gitlab# docker start f9   
f9
[root@MyCentOS]/usr/local/docker/gitlab# docker ps
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS                            PORTS                                                             NAMES
f9f24d76019a        twang2218/gitlab-ce-zh:10.5   "/assets/wrapper"        About an hour ago   Up 5 seconds (health: starting)   0.0.0.0:80->80/tcp, 0.0.0.0:2222->22/tcp, 0.0.0.0:8443->443/tcp   gitlab_gitlab_1
d473292982f8        tomcat                        "catalina.sh run"        8 days ago          Up 4 hours                        0.0.0.0:8081->8080/tcp                                            web
99000feb760b        mysql:5.7.22                  "docker-entrypoint.s…"   8 days ago          Up About an hour                  0.0.0.0:3307->3306/tcp                                            mysql
[root@MyCentOS]/usr/local/docker/gitlab# 
~~~



## 参考资料
> https://hub.docker.com/r/twang2218/gitlab-ce-zh
> https://www.bilibili.com/video/av29384041/?p=48
> https://www.funtl.com/zh/gitlab/%E5%9F%BA%E4%BA%8E-Docker-%E5%AE%89%E8%A3%85-GitLab.html
> http://www.mamicode.com/info-detail-2230071.html
