---
title: Vue学习03-生命4ue布局
catalog: true
date: 2019-08-17 10:29:40
subtitle:
header-img:
tags: [Vue]
---

# Vue学习记录
这边博客为自己学习vue的记录.开始篇幅较大,如果基础好的同学可以直接点击vue内容开始浏览.

## Vue布局
### 表单输出
什么是双向数据绑定
Vue.js 是一个 MVVM 框架，即数据双向绑定，即当数据发生变化的时候，视图也就发生变化，当视图发生变化的时候，数据也会跟着同步变化。这也算是 Vue.js 的精髓之处了。值得注意的是，我们所说的数据双向绑定，一定是对于 UI 控件来说的，非 UI 控件不会涉及到数据双向绑定。单向数据绑定是使用状态管理工具的前提。如果我们使用 vuex，那么数据流也是单项的，这时就会和双向数据绑定有冲突。

### 为什么要实现数据的双向绑定
在 Vue.js 中，如果使用 vuex，实际上数据还是单向的，之所以说是数据双向绑定，这是用的 UI 控件来说，对于我们处理表单，Vue.js 的双向数据绑定用起来就特别舒服了。即两者并不互斥，在全局性数据流使用单项，方便跟踪；局部性数据流使用双向，简单易操作。

### 在表单中使用双向数据绑定
你可以用 v-model 指令在表单 <input>、<textarea> 及 <select> 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

注意：v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。

~~~ vue
<div id="vue">
    单行文本：<input type="text" v-model="message" />&nbsp;&nbsp;单行文本是：{{message}}
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            message: "Hello Vue"
        }
    });
</script>
~~~
**输出**


## 参考资料

> 李卫民的教程
> https://www.funtl.com/zh/vue-prepare/
> https://www.bilibili.com/video/av44230028/?p=1
> Vue官网教程
> https://cn.vuejs.org/v2/guide/installation.html
