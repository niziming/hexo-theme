---
title: Vue学习03-生命周期函数
catalog: true
date: 2019-08-17 10:25:22
subtitle:
header-img:
tags: [Vue]
---

# Vue学习记录
这边博客为自己学习vue的记录.开始篇幅较大,如果基础好的同学可以直接点击vue内容开始浏览.

### Vue生命周期 
- created: 比如钩子可以用来在一个实例被创建之后执行代码：
- mounted
- updated
- destroyed 

每个 Vue 实例在被创建时都要经过一系列的初始化过程 —— 例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。
![vueLifeLoop](Vue学习03-生命周期函数/vueLifeLoop.jpg)
比如 created 钩子可以用来在一个实例被创建之后执行代码：
~~~ vue
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
~~~
也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 mounted、updated 和 destroyed。
生命周期钩子的 this 上下文指向调用它的 Vue 实例。

###钩子函数的触发时机
#### beforeCreate
在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。

#### created
实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。

#### beforeMount
在挂载开始之前被调用：相关的 render 函数首次被调用。

#### mounted
el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。

#### beforeUpdate
数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

#### updated
由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。

#### beforeDestroy
实例销毁之前调用。在这一步，实例仍然完全可用。

#### destroyed
Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。

## 参考资料

> 李卫民的教程
> https://www.funtl.com/zh/vue-prepare/
> https://www.bilibili.com/video/av44230028/?p=1
> Vue官网教程
> https://cn.vuejs.org/v2/guide/installation.html
