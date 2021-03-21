---
title: Vue学习02-Vue语法
catalog: true
date: 2019-08-17 10:19:12
subtitle:
header-img:
tags: [Vue]
---

# Vue学习记录
这边博客为自己学习vue的记录.开始篇幅较大,如果基础好的同学可以直接点击vue内容开始浏览.

## Vue 语法
- `v-if`
~~~ vue
    <div id="app">
      <p v-if="a === 'a'">a</p>
      <p v-if="a === 'b'">b</p>
      <p v-if="a === 'c'">c</p>
    </div>
   <script>
         var vm = new Vue({
           el:"#app",
           data:{
             a: "b",
           }
         })
   </script> 
~~~

输出
~~~ 
b
~~~

- `v-for`
~~~ vue
<div id="app">
      <ul>
        <li v-for="item in items">
          {{item}}
        </li>
      </ul>
    </div>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
          items: ['张三', '李四', '王麻子']
        }
      })
    </script>
~~~
输出
![v-for](Vue学习02-Vue语法/v-for.png)

- `v-on`
~~~ vue
<div id="app">
      <button v-on:click="say()">
        点击弹出
      </button>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      var vm = new Vue({
        el:"#app",
        methods: {
          say: function (event){
            alert("hello vue")
          }
        }
      })
    </script>
~~~
输出
![v-on](Vue学习02-Vue语法/v-on.png)

第二种
- `v-on`
~~~ vue
<div id="app">
      <button v-on:click="say()">
        点击弹出
      </button>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      var vm = new Vue({
        el:"#app",
        data:{
          msg:"hello vue[msg]"
        },
        methods: {
          say: function (event){
            alert(this.msg)
          }
        }
      })
    </script>
~~~
输出
![v-on1](Vue学习02-Vue语法/v-on1.png)

## 参考资料

> 李卫民的教程
> https://www.funtl.com/zh/vue-prepare/
> https://www.bilibili.com/video/av44230028/?p=1
> Vue官网教程
> https://cn.vuejs.org/v2/guide/installation.html
