---
title: Vue学习01-安装及使用
catalog: true
date: 2019-08-16 09:49:24
subtitle:
header-img:
tags: [Vue]
---

# Vue学习记录
这边博客为自己学习vue的记录.开始篇幅较大,如果基础好的同学可以直接点击vue内容开始浏览.

### 什么是Vue.js
- Vue.js 是目前最火的前端框架,Vue.js也是可以进行手机app开发的需要借助于Weex

- React是最流行的前端框架(网站和手机app都可以开发)

- Vue Angular React是目前前端三大主流框架!

- Vue是一套构建用户界面的框架, **只关注视图层**

- 前端工作 为mvc的view层

# Vue的学习
**Vue学习的内容开始.**
Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的**渐进式框架**，
发布于 2014 年 2 月。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。
Vue 的核心库只**关注视图层**，不仅易于上手，还便于与第三方库
（如：vue-router，vue-resource，vuex）或既有项目整合。
- 优雅降级: 向下兼容
- 渐进增强: 内容为主,更具浏览器版本的不同,增加特效.向上兼容

- Vue-router :前后端分离, 后台不可干涉前端功能,需要router做路由.
- Vuex 状态管理框架;

### 为什么要使用 Vue.js
- 轻量级，体积小是一个重要指标。Vue.js 压缩后有只有 20多kb （Angular 压缩后 56kb+，React 压缩后 44kb+）
- 移动优先。更适合移动端，比如移动端的 Touch 事件
- 易上手，学习曲线平稳，文档齐全
- 吸取了 Angular（模块化）和 React（虚拟 DOM）的长处，并拥有自己独特的功能，如：计算属性
- 开源，社区活跃度高

## Vue.js 的两大核心要素
### 数据驱动
![dataDriver](Vue学习01-安装及使用/dataDriver.png)
当你把一个普通的 JavaScript 对象传给 Vue 实例的 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty 
把这些属性全部转为 getter/setter。Object.defineProperty 是 ES5 中一个无法 shim 的特性，
这也就是为什么 Vue 不支持 IE8 以及更低版本浏览器。

每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知
 watcher 重新计算，从而致使它关联的组件得以更新。

## 安装
- 如果想快速上手,直接引入生产环境的Vue,然后开始测试学习.
~~~ javascript
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
~~~

- 新建一个Vue程序
利用npm 安装vue
~~~ shell
# 最新稳定版
$ npm install vue
~~~
### 命令行工具 (CLI)
`待完善`

### 组件化
- 页面上每个独立的可交互的区域视为一个组件
- 每个组件对应一个工程目录，组件所需的各种资源在这个目录下就近维护
- 页面不过是组件的容器，组件可以嵌套自由组合（复用）形成完整的页面

## 第一个Vue程序
~~~ vue
<!DOCTYPE html>
<html lang="zh">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>my-vue</title>
  </head>
  <body>
    <div id="app">
      {{a}}
    </div>

    <!-- built files will be auto injected -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      var vm = new Vue({
        el:"#app",
        data:{
          a: "test"
        }
      })
    </script>
  </body>
</html>
~~~

## 参考资料

> 李卫民的教程
> https://www.funtl.com/zh/vue-prepare/
> https://www.bilibili.com/video/av44230028/?p=1
> Vue官网教程
> https://cn.vuejs.org/v2/guide/installation.html
