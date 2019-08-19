---
title: Vue
catalog: true
date: 2019-08-15 09:49:24
subtitle:
header-img:
tags: [Vue]
---

# Vue学习记录
这边博客为自己学习vue的记录.

## 学习之前了解

### 什么是Vue.js
- Vue.js 是目前最火的前端框架,Vue.js也是可以进行手机app开发的需要借助于Weex

- React是最流行的前端框架(网站和手机app都可以开发)

- Vue Angular React是目前前端三大主流框架!

- Vue是一套构建用户界面的框架, **只关注视图层**

- 前端工作 为mvc的view层

### 框架和库之间的区别
- 框架: 是一套完整的解决方案, 对项目的侵入性教大.

- 库(插件): 提供一个小的功能, 对项目侵入性笑.

### 前端知识体系
- html(结构层) 骨架
- css(表现层) 特效: 缺陷标记语言,没有变量.没一个特效需要一个css标记.
    - css预处理器
        - SASS: 基于Ruby, 基于服务器端处理,功能强大,解析高,上手难度高于LESS.
        - LESS: 基于NodeJS,通过客户端处理,使用简单,功能比SASS简单,解析效率也低于SASS.如果后端人员学习可以使用LESS.
- JavaScript(行为层) 交互: 动起来,行为.

### Native(原生的,本地的)JS开发
原生JS开发，也就是让我们按照 【ECMAScript】 标准的开发方式，简称是 ES，特点是所有浏览器都支持。截止到当前博客发布时间（2018 年 12 月 04 日），ES 标准已发布如下版本：
<div id="app"></div>
$("#app") jquery的语法
.append往DOM追加元素
(原生的)document.getElementById("app") 获取dom节点

ECMA欧洲电脑制造商协会（European Computer Manufactures Association）

- ES3
- ES4（内部，未正式发布）
- ES5（全浏览器支持）
- ES6（常用，当前主流版本） 相对于5增加了一些特性
因为不同的标准所以前端可以使用打包工具来打包
- ES7
- ES8
- ES9（草案阶段）.

### TypeScript 微软的标准
TypeScript 是一种由微软开发的自由和开源的编程语言。它是 JavaScript 的一个超集，
而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。由安德斯·海尔斯伯格
（C#、Delphi、TypeScript 之父；.NET 创立者）主导。
该语言的特点就是除了具备 ES 的特性之外还纳入了许多不在标准范围内的新特性，
所以会导致很多浏览器不能直接支持 TypeScript 语法，需要编译后（编译成 JS）
才能被浏览器正确执行。

### js框架
- jQuery：大家熟知的 JavaScript 框架，优点是简化了 DOM 操作，
缺点是 DOM 操作太频繁，影响前端性能；在前端眼里使用它仅仅是为了兼容 IE6、7、8；
- Angular：Google 收购的前端框架，由一群 Java 程序员开发，其特点
是将后台的 MVC 模式搬到了前端并增加了模块化开发的理念，与微软合作，
采用 TypeScript 语法开发；对后台程序员友好，对前端程序员不太友好；
最大的缺点是版本迭代不合理（如：1代 -> 2代开发方式完全改变，除了名字，
基本就是两个东西；截止发表博客时已推出了 Angular6）
- React：Facebook 出品，一款高性能的 JS 前端框架；特点是提出了
新概念 【虚拟 DOM】 用于减少真实 DOM 操作，在内存中模拟 DOM 操作，
有效的提升了前端渲染效率；缺点是使用复杂，因为需要额外学习一门 【JSX】 语言；
- Vue：一款渐进式 JavaScript 框架，所谓渐进式就是逐步实现新特性的意思，
如实现模块化开发、路由、状态管理等新特性。其特点是综合了 Angular（模块化）
 和 React（虚拟 DOM） 的优点；
    - SoC 关注度分离原则
- Axios：前端通信框架；因为 Vue 的边界很明确，就是为了处理 DOM，所以并
不具备通信能力，此时就需要额外使用一个通信框架与服务器交互；当然也可以
直接选择使用 jQuery 提供的 AJAX 通信功能；

### UI框架
- Ant-Design：阿里巴巴出品，基于 React 的 UI 框架
- ElementUI：饿了么出品，基于 Vue 的 UI 框架
- iview: 基于vue框架
- ice: 飞冰
- Bootstrap：Twitter 推出的一个用于前端开发的开源工具包
- AmazeUI：又叫“妹子 UI”，一款 HTML5 跨屏前端框架,也不错

> 前端一般采用`vue + element`框架

### 工具
- WebPack 前端打包工具:可将ES6的新特性转换成ES5语法. 模块打包器，主要作用是打包、压缩、合并及按序加载
通常前端 开发会用ES6标准写,然后用webpack打包
- Gulp 
- Babel JS 编译工具，主要用于浏览器不支持的 ES 新特性，比如用于编译 TypeScript

## 三端同一
### 混合开发（Hybrid App）

主要目的是实现一套代码三端统一（PC、Android、iOS）并能够调用到设备底层硬件（如：传感器、GPS、摄像头等），打包方式主要有以下两种：

.apk这种格式

- 云打包：HBuild -> HBuildX，DCloud 出品；API Cloud
- 本地打包：需要搭建环境 Cordova（前身是 PhoneGap） 打包苹果的需要在ios平台

### 中间件
软件调用中间件, 中间件调用底层硬件.

### 微信小程序
详见微信官网，这里就是介绍一个方便微信小程序 UI 开发的框架：WeUi

## 后端技术
前端人员为了方便开发也需要掌握一定的后端技术，但我们 Java 后台人员知道后台知识体系极其庞大复杂，所以为了方便前端人员开发后台应用，就出现了 NodeJS 这样的技术。

NodeJS 的作者已经声称放弃 NodeJS（说是架构做的不好再加上笨重的 node_modules，可能让作者不爽了吧），开始开发全新架构的 Deno

既然是后台技术，那肯定也需要框架和项目管理工具，NodeJS 框架及项目管理工具如下：

- Express：NodeJS 框架
- Koa：Express 简化版
- NPM：项目综合管理工具，类似于 Maven 编译打包运行下载依赖,js依赖
- YARN：NPM 的替代方案，类似于 Maven 和 Gradle 的关系
    - Gradle和maven一样但效率更高

#### 附：当前主流前端框架
### Vue.js
### iView
iview 是一个强大的基于 Vue 的 UI 库，有很多实用的基础组件比 elementui 的组件更丰富，主要服务于 PC 界面的中后台产品。使用单文件的 Vue 组件化开发模式 基于 npm + webpack + babel 开发，支持 ES2015 高质量、功能丰富 友好的 API ，自由灵活地使用空间。

官网地址
Github
iview-admin
备注：属于前端主流框架，选型时可考虑使用，主要特点是移动端支持较多

### ElementUI
Element 是饿了么前端开源维护的 Vue UI 组件库，组件齐全，基本涵盖后台所需的所有组件，文档讲解详细，例子也很丰富。主要用于开发 PC 端的页面，是一个质量比较高的 Vue UI 组件库。

官网地址
Github
vue-element-admin
备注：属于前端主流框架，选型时可考虑使用，主要特点是桌面端支持较多

### ICE
飞冰是阿里巴巴团队基于 React/Angular/Vue 的中后台应用解决方案，在阿里巴巴内部，已经有 270 多个来自几乎所有 BU 的项目在使用。飞冰包含了一条从设计端到开发端的完整链路，帮助用户快速搭建属于自己的中后台应用。

官网地址
Github
备注：主要组件还是以 React 为主，截止 2019 年 02 月 17 日更新博客前对 Vue 的支持还不太完善，目前尚处于观望阶段

### VantUI
Vant UI 是有赞前端团队基于有赞统一的规范实现的 Vue 组件库，提供了一整套 UI 基础组件和业务组件。通过 Vant，可以快速搭建出风格统一的页面，提升开发效率。

官网地址
Github
### AtUI
at-ui 是一款基于 Vue 2.x 的前端 UI 组件库，主要用于快速开发 PC 网站产品。 它提供了一套 npm + webpack + babel 前端开发工作流程，CSS 样式独立，即使采用不同的框架实现都能保持统一的 UI 风格。

官网地址
Github
### CubeUI
cube-ui 是滴滴团队开发的基于 Vue.js 实现的精致移动端组件库。支持按需引入和后编译，轻量灵活；扩展性强，可以方便地基于现有组件实现二次开发。

官网地址
Github
### 混合开发
### Flutter
Flutter 是谷歌的移动端 UI 框架，可在极短的时间内构建 Android 和 iOS 上高质量的原生级应用。Flutter 可与现有代码一起工作, 它被世界各地的开发者和组织使用, 并且 Flutter 是免费和开源的。

官网地址
Github
备注：Google 出品，主要特点是快速构建原生 APP 应用程序，如做混合应用该框架为必选框架

### Ionic
Ionic 既是一个 CSS 框架也是一个 Javascript UI 库，Ionic 是目前最有潜力的一款 HTML5 手机应用开发框架。通过 SASS 构建应用程序，它提供了很多 UI 组件来帮助开发者开发强大的应用。它使用 JavaScript MVVM 框架和 AngularJS/Vue 来增强应用。提供数据的双向绑定，使用它成为 Web 和移动开发者的共同选择。

官网地址
官网文档
Github
### 微信小程序
### mpvue
mpvue 是美团开发的一个使用 Vue.js 开发小程序的前端框架，目前支持 微信小程序、百度智能小程序，头条小程序 和 支付宝小程序。 框架基于 Vue.js，修改了的运行时框架 runtime 和代码编译器 compiler 实现，使其可运行在小程序环境中，从而为小程序开发引入了 Vue.js 开发体验。

官网地址
Github
备注：完备的 Vue 开发体验，并且支持多平台的小程序开发，推荐使用

### WeUI
WeUI 是一套同微信原生视觉体验一致的基础样式库，由微信官方设计团队为微信内网页和微信小程序量身设计，令用户的使用感知更加统一。包含 button、cell、dialog、toast、article、icon 等各式元素。

官网地址
Github


## 前后分离的演变史
### 为什么需要前后分离
### 后端为主的 MVC 时代
为了降低开发的复杂度，以后端为出发点，比如：Struts、SpringMVC 等框架的使用，
就是后端的 MVC 时代;可以参考 【什么是 MVC 模式】

![](mvc.png)

### 什么是前后分离
#### 基于 AJAX 带来的 SPA 时代
时间回到 2005 年 AJAX（Asynchronous JavaScript And XML，异步 JavaScript
 和 XML，老技术新用法） 被正式提出并开始使用 CDN 作为静态资源存储，
 于是出现了 JavaScript 王者归来（在这之前 JS 都是用来在网页上贴狗皮膏药广告的）的 SPA
 （Single Page Application）单页面应用时代。
 
![](web.png)

### 前端为主的 MV* 时代
此处的 MV* 模式如下：

MVC（同步通信为主）：Model、View、Controller
MVP（异步通信为主）：Model、View、Presenter
MVVM（异步通信为主）：Model、View、ViewModel
为了降低前端开发复杂度，涌现了大量的前端框架，比如：AngularJS、React、
Vue.js、EmberJS 等，这些框架总的原则是先按类型分层，比如 Templates、
Controllers、Models，然后再在层内做切分，如下图：

优点
- 前后端职责很清晰： 前端工作在浏览器端，后端工作在服务端。清晰的分工，可以让开发并行，测试数据的模拟不难，前端可以本地开发。后端则可以专注于业务逻辑的处理，输出 RESTful（可以参考 【如何设计一个良好的 API】）等接口。
- 前端开发的复杂度可控： 前端代码很重，但合理的分层，让前端代码能各司其职。这一块蛮有意思的，简单如模板特性的选择，就有很多很多讲究。并非越强大越好，限制什么，留下哪些自由，代码应该如何组织，所有这一切设计，得花一本书的厚度去说明。
- 部署相对独立： 可以快速改进产品体验

缺点
- 代码不能复用。比如后端依旧需要对数据做各种校验，校验逻辑无法复用浏览器端的代码。如果可以复用，那么后端的数据校验可以相对简单化。
- 全异步，对 SEO 不利。往往还需要服务端做同步渲染的降级方案。
- 性能并非最佳，特别是移动互联网环境下。
- SPA 不能满足所有需求，依旧存在大量多页面应用。URL Design 需要后端配合，前端无法完全掌控.

### NodeJS 带来的(前端)全栈时代
前端为主的 MV* 模式解决了很多很多问题，但如上所述，依旧存在不少不足之处。随着 NodeJS 的兴起，
JavaScript 开始有能力运行在服务端。这意味着可以有一种新的研发模式：

front-end -> nodejs -> back-end 实现了真正的前后分离

在这种研发模式下，前后端的职责很清晰。对前端来说，两个 UI 层各司其职：
- Front-end UI layer 处理浏览器层的展现逻辑。通过 CSS 渲染样式，通过 JavaScript 添加交互功能，
HTML 的生成也可以放在这层，具体看应用场景。
- Back-end UI layer 处理路由、模板、数据获取、Cookie 等。通过路由，前端终于可以自主把控 URL Design，
这样无论是单页面应用还是多页面应用，前端都可以自由调控。后端也终于可以摆脱对展现的强关注，
转而可以专心于业务逻辑层的开发。

**注：看到这里，相信很多同学就可以理解，为什么我总在课堂上说：“前端想学后台很难，而我们后端程序员学
任何东西都很简单”；就是因为我们后端程序员具备相对完善的知识体系。**

## MVVM模式
什么是 MVVM
MVVM（Model-View-ViewModel）是一种软件架构设计模式，由微软 WPF（用于替代 WinForm，以前就是用这个
技术开发桌面应用程序的）和 Silverlight（类似于 Java Applet，简单点说就是在浏览器上运行的 WPF） 
的架构师 Ken Cooper 和 Ted Peters 开发，是一种简化用户界面的**事件驱动编程方式**。由 John Gossman
（同样也是 WPF 和 Silverlight 的架构师）于 2005 年在他的博客上发表。

MVVM 源自于经典的 MVC（Model-View-Controller）模式（期间还演化出了 MVP（Model-View-Presenter）
 模式）。MVVM 的核心是 ViewModel 层，负责转换 Model 中的数据对象来让数据变得更容易管理和使用，
 其作用如下：

- 该层向上与视图层进行双向数据绑定
- 向下与 Model 层通过接口请求进行数据交互
![](mvvm.png)

## 为什么要使用 MVVM
MVVM 模式和 MVC 模式一样，主要目的是分离视图（View）和模型（Model），有几大好处

- 低耦合： 视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的 View 上，
当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。
- 可复用： 你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 View 重用这段视图逻辑。
- 独立开发： 开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计。
- 可测试： 界面素来是比较难于测试的，而现在测试可以针对 ViewModel 来写。

MVVM 的组成部分


### View
View 是视图层，也就是用户界面。前端主要由 HTML 和 CSS 来构建，为了更方便地展现 ViewModel 或者 Model 层的数据，已经产生了各种各样的前后端模板语言，比如 FreeMarker、Thymeleaf 等等，各大 MVVM 框架如 Vue.js，AngularJS，EJS 等也都有自己用来构建用户界面的内置模板语言。

### Model
Model 是指数据模型，泛指后端进行的各种业务逻辑处理和数据操控，主要围绕数据库系统展开。这里的难点主要在于需要和前端约定统一的 接口规则

### ViewModel
ViewModel 是由前端开发人员组织生成和维护的视图数据层。在这一层，前端开发者对从后端获取的 Model 数据进行转换处理，做二次封装，以生成符合 View 层使用预期的视图数据模型。

需要注意的是 ViewModel 所封装出来的数据模型包括视图的状态和行为两部分，而 Model 层的数据模型是只包含状态的

比如页面的这一块展示什么，那一块展示什么这些都属于视图状态（展示）
页面加载进来时发生什么，点击这一块发生什么，这一块滚动时发生什么这些都属于视图行为（交互）
视图状态和行为都封装在了 ViewModel 里。这样的封装使得 ViewModel 可以完整地去描述 View 层`。由于实现了双向绑定，ViewModel 的内容会实时展现在 View 层，这是激动人心的，因为前端开发者再也不必低效又麻烦地通过操纵 DOM 去更新视图。

- MVVM 框架已经把最脏最累的一块做好了，我们开发者只需要处理和维护 ViewModel，更新数据视图就会自动得到相应更新，真正实现 事件驱动编程。
- View 层展现的不是 Model 层的数据，而是 ViewModel 的数据，由 ViewModel 负责与 Model 层交互，这就完全解耦了 View 层和 Model 层，这个解耦是至关重要的，它是前后端分离方案实施的重要一环。

## Vue的学习
**Vue学习的内容开始.**
Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的**渐进式框架**，
发布于 2014 年 2 月。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。
Vue 的核心库只**关注视图层**，不仅易于上手，还便于与第三方库
（如：vue-router，vue-resource，vuex）或既有项目整合。
- 优雅降级: 向下兼容
- 渐进增强: 内容为主,更具浏览器版本的不同,增加特效.向上兼容

- Vue-router :前后端分离, 后台不可干涉前端功能,需要router做路由.
- Vuex 状态管理框架;

## MVVM 模式的实现者

我们知道MVVM 表示如下：

- Model：模型层，在这里表示 JavaScript 对象
- View：视图层，在这里表示 DOM（HTML 操作的元素）
- ViewModel：连接视图和数据的中间件，Vue.js 就是 MVVM 中的 ViewModel 层的实现者

![](mvvmVue.png)

在 MVVM 架构中，是不允许 数据 和 视图 直接通信的，只能通过 ViewModel 来通信，而 ViewModel 就是定义了一个 Observer 观察者

- ViewModel 能够观察到数据的变化，并对视图对应的内容进行更新
- ViewModel 能够监听到视图的变化，并能够通知数据发生改变
至此，我们就明白了，Vue.js 就是一个 MVVM 的实现者，他的核心就是实现了 DOM 监听 与 数据绑定


### 为什么要使用 Vue.js
- 轻量级，体积小是一个重要指标。Vue.js 压缩后有只有 20多kb （Angular 压缩后 56kb+，React 压缩后 44kb+）
- 移动优先。更适合移动端，比如移动端的 Touch 事件
- 易上手，学习曲线平稳，文档齐全
- 吸取了 Angular（模块化）和 React（虚拟 DOM）的长处，并拥有自己独特的功能，如：计算属性
- 开源，社区活跃度高

## Vue.js 的两大核心要素
### 数据驱动
![](dataDriver.png)
当你把一个普通的 JavaScript 对象传给 Vue 实例的 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty 
把这些属性全部转为 getter/setter。Object.defineProperty 是 ES5 中一个无法 shim 的特性，
这也就是为什么 Vue 不支持 IE8 以及更低版本浏览器。

每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知
 watcher 重新计算，从而致使它关联的组件得以更新。

组件化
- 页面上每个独立的可交互的区域视为一个组件
- 每个组件对应一个工程目录，组件所需的各种资源在这个目录下就近维护
- 页面不过是组件的容器，组件可以嵌套自由组合（复用）形成完整的页面

### Vue生命周期 
- created: 比如钩子可以用来在一个实例被创建之后执行代码：
- mounted
- updated
- destroyed 

## 参考资料
> https://www.funtl.com/zh/vue-prepare/
> https://www.bilibili.com/video/av44230028/?p=1
