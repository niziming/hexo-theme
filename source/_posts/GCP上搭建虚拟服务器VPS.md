---
title: GCP上搭建虚拟服务器VPS
catalog: true
tags: [GCP]
date: 2021-03-20 11:10:24
subtitle:
header-img:
---

# 在GCP上搭建虚拟服务器VPS
## GCP注册的准备工作
- 需要VISA/MasterCard实体信用卡
- 在Google cloud platform上申请领取$300促销赠金
- 填写账单信息, 可根据谷歌地图查找VPN真实地址相对应

> 促销赠金可用作一种付款方式，减少您应支付的总金额。GCP 免费试用和营销活动赠金归入此类别。

## 创建一个虚拟服务器

我注册GCP时使用的香港地区的IP, 这里建议搭建台湾地区, 性能选择最低的,这样每个月费用8.08港币.

![image-20210320114607638](GCP上搭建虚拟服务器VPS/image-20210320114607638.png)

## 创建防火墙规则

VPS初始化完成后需要修改创建防火墙规则,一下图片为我的配置图方便后面搭建服务,数据库,docker暴露端口使用.

![image-20210320114913928](GCP%E4%B8%8A%E6%90%AD%E5%BB%BA%E8%99%9A%E6%8B%9F%E6%9C%8D%E5%8A%A1%E5%99%A8VPS/image-20210320114913928.png)

## 使用shell连接服务器

这里使用的xshell



![image-20210320115328701](GCP%E4%B8%8A%E6%90%AD%E5%BB%BA%E8%99%9A%E6%8B%9F%E6%9C%8D%E5%8A%A1%E5%99%A8VPS/image-20210320115328701.png)

没有ssh-key的可以使用xshell自带的工具生成一个

## 上传SSH公钥到服务器

![image-20210320115617696](GCP%E4%B8%8A%E6%90%AD%E5%BB%BA%E8%99%9A%E6%8B%9F%E6%9C%8D%E5%8A%A1%E5%99%A8VPS/image-20210320115617696.png)

把生成的ssh-key 保存服务器创建ssh秘钥处

<!--如果上一步忘了可以在元配置处配置-->

## 搭建成功

![image-20210320122741827](GCP%E4%B8%8A%E6%90%AD%E5%BB%BA%E8%99%9A%E6%8B%9F%E6%9C%8D%E5%8A%A1%E5%99%A8VPS/image-20210320122741827.png)