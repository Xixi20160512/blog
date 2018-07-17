---
title: react-native入门
date: 2018-07-13 15:17:42
tags: ['react-native', '培训']
---

## 简介

> Build native mobile apps using JavaScript and React

- 原生app体验

    使用rn构建的应用，最终会以原生平台的UI组件在android或者ios设备上运行。

- 开发阶段热重载

    体验`ctrl + s`之后刷新UI的快感

- 混合原生代码能力

    说实话，这部分我也没有涉足很深。

## 文档以及学习资源

* 官网地址[react-native](https://facebook.github.io/react-native/)
* 非官网中文网[react-native-cn](https://reactnative.cn/)
* [《深入react技术栈》](https://book.douban.com/subject/26918038/)
* [es6入门](http://es6.ruanyifeng.com/)

ps: 中文网站会存在文档翻译出错的问题，以英文官网为主，且需要注意版本号（截止目前rn最新版本为0.56）。

## 进入主题

以下内容会结合具体项目讨论对rn的实践过程中遇到的问题以及经验。

### 安装
[安装教程](https://facebook.github.io/react-native/docs/getting-started)

需要注意的是：
- 两种不同的安装方式
    * create-react-native-app初始化的项目是不包含android以及ios文件夹的，也就是意味着不予许混合原生代码开发。
    * react-native相反，得到的是一个完全体的项目，通过``ejected``可以将前者转换成完全体。
- 针对于ios模拟器的切换
    * 使用指定``simulator``的方式声明需要使用的模拟器

### 编码
进入编码之前，要搞清楚react-native和react的关系。react官方给出的说明是：
> A JavaScript library for building user interfaces

从这个官方说明中，至少可以看出以下两点：
- react是一个用来构建ui的库（甚至没有说自己是一个框架）
- 没有指定平台（当然默认的是web环境）

那么，具体来说react是用来干嘛的？原理又是什么？我的理解是这样的：以``JSX``语法构建组件，然后以``Viturl DOM``的方式将所有元素组成树状的结构，最后渲染成实际的UI元素。

基于react的这些特点，react-native平台至少做了以下两点改变：
- 提供原生组件的支持
- 将虚拟组件树渲染成原生平台的组件

下面抛开react与react-native之间的差异，仅仅分析其中的核心思想。

#### 特殊的jsx语法
> jsx语法较为纯粹的javascript风格，不失为模板界的一朵奇葩。

熟悉vue或者angular的应该对它们在模板中使用变量以及绑定逻辑的方式很了解了，总结一下就是，自己定义一套解析的规则，按照规则来办事就可以。比如说：
```javascript
<com index="nihao" :value="nihao"/>
```
就是表达的前一个绑定的prop，值为字符串``nihao``，后一个则是变量``nihao``。在react的世界纯粹很多。以下：
```javascript
<com index="nihao" value={nihao} />
```
作用和前者一样，分别绑定字符串和变量，看起来好像差不多，而且就是一个用冒号，一个用花括号，半斤八两！？其实不然，在jsx的世界中只需要记住：``<``开头的是普通模板，会当做字符串解析，而以``{``开头的则会被理解为表达式，会受到相关变量的影响。所以在jsx里面，可以这样写：
```javascript
<com>
{
    nihaoma ? (
        <span>me too!</span>
    ) : (
        <span>very sorry to hear that~</span>
    )
}
</com>
```
是不是看起来很复杂，是很复杂！但是纯粹，纯粹的东西往往更酷。更详细的jsx文档，请[移步](https://reactjs.org/docs/introducing-jsx.html)。

#### 视图刷新
虽然react号称``构建UI的库``，但绝大多数时候我们还是把他当做一个框架，因为react也提供了一个刷新视图的方式，对于一个``MVVM``框架来说，已经很够格了。这个方式的载体就是state对象。看例子：
<p data-height="265" data-theme-id="0" data-slug-hash="wxMXQP" data-default-tab="js,result" data-user="Xixi20160512" data-embed-version="2" data-pen-title="react-state" class="codepen">See the Pen <a href="https://codepen.io/Xixi20160512/pen/wxMXQP/">react-state</a> by ruanbo (<a href="https://codepen.io/Xixi20160512">@Xixi20160512</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

需要特别注意以下几点：
* 定义state的位置，以及``es6``的语法知识
* 和``Vue``不一样，需要使用``setState``更新数据
* ``setState``方法一定是异步的吗

以下是这一过程的简单描述：
![react-state](http://on-img.com/chart_image/5b4c177ce4b0a6efd48286ac.png)

#### 组件

结合上面的介绍，现在可以知道一个``react``应用其实就是由一个个组件组合起来的，其本质就是一个树状的数据结构（计算机世界中，喜欢使用树状结构）。那么怎么定义以及使用组件呢？具体看下面的例子。
<p data-height="265" data-theme-id="0" data-slug-hash="rrxPKo" data-default-tab="js,result" data-user="Xixi20160512" data-embed-version="2" data-pen-title="react-component" class="codepen">See the Pen <a href="https://codepen.io/Xixi20160512/pen/rrxPKo/">react-component</a> by ruanbo (<a href="https://codepen.io/Xixi20160512">@Xixi20160512</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

以上例子表达的组件之间的关系就是下图这样的：
![react-component](http://on-img.com/chart_image/5b4c57f0e4b054aa54b8dfc3.png)
除了这些基础的认知外，关于组件还需要理解诸如``生命周期``、``组件通讯``等等（比如说高阶组件、纯函数组件，这些可以在《深入React技术栈》一书中学习到）概念。

* 生命周期
![react-lifecircle](http://on-img.com/chart_image/5b4c84f4e4b00b08ad1eb8b6.png)

需要注意的是，与之前版本相比，去除了几个生命周期API，可参考[React-Component文档](https://reactjs.org/docs/react-component.html)

* 组件通讯

组件之间通讯是萦绕在单页应用上永久的话题，除了最原始的``props+event``传递，``react``自身也在一直改变，比如最新的``context api``。社区中也有很多实践方案，比如说``redux``、``mobx``等等。我觉得熟练掌握其中功能强大、概念简单的``mobx``就好了。包括``redux``作者也推荐过这个。

首先看下mobx的定义

> 简单、可扩展的状态管理工具


用一张较为经典的图片来描述一下，整个``mobx``处理的过程。
![mobx-flow](https://mobx.js.org/docs/flow.png)
mobx将应用状态和使用状态的UI、处理数据的方法、网络请求等等操作进行绑定，使用状态的地方将在状态改变的时候自动获取最新的状态。在mobx中的概念相当少，大概有下面这些：
* 被观察的状态
* 计算后的值
* 观察者

实际上，作为一个专门处理应用状态的库，是不限制与某一个框架进行结合的。如果要用在``react``中，就得使用``mobx-react``作为桥梁。这个桥梁起的作用就是将react组件包装成观察者，响应状态的更改。以下是一个简单的例子，将上面组件例子中使用``state+props``的通信方式换成``mobx``。

<p data-height="265" data-theme-id="0" data-slug-hash="JBXpaq" data-default-tab="js,result" data-user="Xixi20160512" data-embed-version="2" data-pen-title="react-component-mobx" class="codepen">See the Pen <a href="https://codepen.io/Xixi20160512/pen/JBXpaq/">react-component-mobx</a> by ruanbo (<a href="https://codepen.io/Xixi20160512">@Xixi20160512</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 总结

react提供了简单、直观地构建UI的能力，为模板带来了动态编程的能力，结合状态管理工具mobx，可以极为简单的构建出一个很棒的应用。与此同时，react自身的高度扩展能力，使得移动端开发也可以被js实现。但是由于时间以及个人的经验所限，本文的内容可能较为浅显、也流于泛泛。技术是好的，平时的积累和总结至关重要，希望这次能和大家一起入这个门，迈进更高的技术殿堂。
PS: 最后附上一个``react-native-todo-demo``的[地址](https://git.fpi-inc.com/fe/react-native-todo)，可以试着跑一下。

