---
title: react-native-learning.md
date: 2018-07-13 15:17:42
tags: ['react-native', '培训']
---

### 简介
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
虽然react号称``构建UI的库``，但绝大多数时候我们还是把他当做一个框架，因为react也提供了一个刷新视图的方式，对于一个``MVVM``框架来说，已经很够格了。这个方式的载体就是state对象。

``to be continued``