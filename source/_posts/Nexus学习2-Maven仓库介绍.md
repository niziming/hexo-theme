---
title: Nexus学习2-Maven仓库介绍
catalog: true
tags: []
date: 2019-10-23 11:10:36
subtitle:
header-img:
---
# Nexus学习2-Maven仓库介绍

## 代理仓库（Proxy Repository）
-意为第三方仓库，如：
    -maven-central
    -nuget.org-proxy
-版本策略（Version Policy）：
    -Release: 正式版本
    -Snapshot: 快照版本
    -Mixed: 混合模式
-布局策略（Layout Policy）：
    -Strict：严格
    -Permissive：宽松
## 宿主仓库（Hosted Repository）
存储本地上传的组件和资源的，如：

- maven-releases
- maven-snapshots
- nuget-hosted
部署策略（Deployment Policy）：

- Allow Redeploy：允许重新部署
- Disable Redeploy：禁止重新部署
- Read-Only：只读

## 仓库组（Repository Group）
通常包含了多个代理仓库和宿主仓库，在项目中只要引入仓库组就可以下载到代理仓库和宿主仓库中的包，如：

- maven-public
- nuget-group

![1](Nexus学习2-Maven仓库介绍/1.png)

## 参考资料
> https://www.funtl.com/zh/nexus/%E5%9C%A8%E9%A1%B9%E7%9B%AE%E4%B8%AD%E4%BD%BF%E7%94%A8-Maven-%E7%A7%81%E6%9C%8D.html#%E9%85%8D%E7%BD%AE%E8%AE%A4%E8%AF%81%E4%BF%A1%E6%81%AF
> https://www.bilibili.com/video/av29384041/?p=53&t=106
