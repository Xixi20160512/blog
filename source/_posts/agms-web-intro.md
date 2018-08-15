---
title: agms-web介绍
date: 2018-08-16 00:16:13
tags: ['vue', 'agms']
---

## 准备阶段

确保以下软件已正确安装：
- nodejs >v8.x
- yarn
- vscode
- git/gitbash

ps: 
* windows建议使用gitbash作为命令行工具
* vscode可以使用配置同步插件来确保编辑器配置的一致性

## 启动项目

下载源码
``` bash
cd path/to/code //进入存放源码的路径
git clone git@git.fpi-inc.site:product/agms/agms-web.git //克隆项目
```
本地调试
``` bash
yarn
cp env/env.dev.js src/env.js //拷贝将要使用的配置文件
yarn start
```
项目成功启动之后会跳转到登陆地址，登陆之后拷贝token。手动模拟跳转的过程。也就是浏览器输入：
```
http://127.0.0.1:8080?token=<token>
```

## 技术栈

* 框架：[Vue.js](https://cn.vuejs.org/index.html)
* UI组件库：[element-ui v1](http://element-cn.eleme.io/1.4/#/zh-CN/)
* ES版本：[es6](http://es6.ruanyifeng.com/)
* 构建工具：[webpack v3](https://webpack.js.org/)
* 样式预编译语言：[stylus](http://stylus-lang.com/)
* 模板语言：[pug](https://pugjs.org/api/getting-started.html)

以上为项目中主要使用的技术栈，其中核心部分为``vuejs``、``element-ui``、``es6``，必须牢牢掌握。其他为辅助或者拔高项，可以深入了解，尤其是``webpack``工具在构建调优。

## 前端整体架构
![agms-web-intro](http://on-img.com/chart_image/5b745729e4b067df5a0b57fa.png)

## 列举一些坑

- ``prop``第一次传值的时候，相应``watcher``不会响应变化
- ``data``中定义的数组，vue只会做一个``浅比较``，也即只有该数组引用改变之后，才会响应变化。(参见：[数组更新检测](https://cn.vuejs.org/v2/guide/list.html#%E6%95%B0%E7%BB%84%E6%9B%B4%E6%96%B0%E6%A3%80%E6%B5%8B))
- ``data``中定义的对象中没有声明的键，在后期操作这个键相应的值的时候，不会响应变化。（computed和watch都不可以）
- 单文件组件总在export之外声明的变量是属于全局的，组件销毁的时候不会自动释放，同时在下一次组件初始化的时候，会引用同一份副本，造成意料之外的bug
- 由于安全的原因，在vue-router中，没有一个接口可以访问到路由跳转的栈，折中的办法是在路由跳转的钩子函数
- ``activated``执行在``beforerouterenter``之前；混合的组件：组件自身的生命周期执行在用于混合的组件之前