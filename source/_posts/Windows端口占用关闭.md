---
title: Windows端口占用关闭
catalog: true
date: 2019-09-24 20:51:43
subtitle:
header-img:
tags: [Windows, Windows端口]
---
# Windows端口占用关闭

~~~shell
# 查看端口占用pid
C:\Users\ziming>netstat -ano|findstr 8080
  TCP    0.0.0.0:8080           0.0.0.0:0              LISTENING       8912
  TCP    [::]:8080              [::]:0                 LISTENING       8912
# 杀死pid
C:\Users\ziming>taskkill/pid 8912 /F
成功: 已终止 PID 为 8912 的进程。
# 再次查看
C:\Users\ziming>netstat -ano|findstr 8080
~~~
