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

![](1.png)

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

![](1.pmg)
![](2.pmg)

再次启动后访问,如图所示
![](3.pmg)
502错误查询资料显示是硬件问题,内存不足所致.
`docker logs -f id`
~~~
...
  * ruby_block[restart postgres-exporter svlogd configuration] action create
    - execute the ruby block restart postgres-exporter svlogd configuration
  * ruby_block[reload postgres-exporter svlogd configuration] action create
    - execute the ruby block reload postgres-exporter svlogd configuration

Running handlers:
Running handlers complete
Chef Client failed. 408 resources updated in 04 minutes 08 seconds
There was an error running gitlab-ctl reconfigure:

execute[clear the gitlab-rails cache] (gitlab::gitlab-rails line 402) had an error: Mixlib::ShellOut::ShellCommandFailed: Expected process to exit with [0], but received ''
---- Begin output of /opt/gitlab/bin/gitlab-rake cache:clear ----
STDOUT: 
STDERR: 
---- End output of /opt/gitlab/bin/gitlab-rake cache:clear ----
Ran /opt/gitlab/bin/gitlab-rake cache:clear returned 
~~~



## 参考资料
> https://hub.docker.com/r/twang2218/gitlab-ce-zh
> https://www.bilibili.com/video/av29384041/?p=48
> https://www.funtl.com/zh/gitlab/%E5%9F%BA%E4%BA%8E-Docker-%E5%AE%89%E8%A3%85-GitLab.html
