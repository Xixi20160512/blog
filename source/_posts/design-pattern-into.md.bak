---
title: javascript 设计模式
date: 2018-11-22 00:16:13
tags: ['javaScript', '设计模式']
---

### 什么是设计模式

> 在面向对象软件设计过程中针对特定问题的简洁而优雅的解决方案。

GoF(Gang Of Four)四人总结了 23 种常见的模式，写成了一本专门介绍设计模式的书。不同的编程语言由于其自身的特性，在实现设计模式的手法上可能存在较大差异性。作为 javaScript 开发人员，有必要学习一下这门语言实现设计模式的方式。

书籍推荐：
![](/blog/images/design-pattern-0.png)

### 常用模式5讲

注意，设计模式主要是针对面向对象的编程模型，同时，也不是所有模式都适合在 javaScript 下实现。再加上还有其他的编程模型（函数式编程等等），所以设计模式可以开拓我们的视野，但是最好不要被“模式”束缚。

PS: 以下内容均来自上面推荐的那本书，如果需要更好的理解，还是建议花时间阅读原著。

#### 单例模式

> 保证一个类仅有一个实例，并提供一个访问它的全局访问点。

基础实现
<p data-height="265" data-theme-id="0" data-slug-hash="KroBZy" data-default-tab="js,result" data-user="Xixi20160512" data-pen-title="singleton-01" class="codepen">See the Pen <a href="https://codepen.io/Xixi20160512/pen/KroBZy/">singleton-01</a> by ruanbo (<a href="https://codepen.io/Xixi20160512">@Xixi20160512</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

只要是能实现全局访问到同一个对象，就可以称为时单例模式。在 js 里面可以直接声明一个全局变量
```
var obj = {}
```
那么 `obj` 就可以理解为是一个单例了。

#### 策略模式

> 定义一系列的算法，把它们封装起来，并且使它们可以相互替换

假设现在有一段计算奖金的代码如下：

``` 
var calculateBonus = function (level, salary) {
    if(level === 'A') {
        return salary * 4
    }
    if(level === 'B') {
        return salary * 3
    }
    if(level === 'C') {
        return salary * 2
    }
}
```
上面的代码肯定每个人都写过，滥用 ``if...else...``分支。。。如果需要再加一个分支，那就更凌乱了，这个时候可以使用策略模式重构：

```
var calculatebonus = function (level, salary) {
    return strategies[level](salary)
}

var strategies = {
    'A': function(salary) {
        return salary * 4
    },
    'B': function(salary) {
        return salary * 3
    },
    'C': function(salary) {
        return salary * 2
    }
}
```
很明显，实现奖金计算的目的是一样的，只需要指明用的是什么“策略”就可以搞定了。后面如果需要更新新的策略，在逻辑上也是很清晰的。

#### 代理模式

> 在需要操作的对象之前加一个中介者，通过对中介者的访问来确定最终对目标对象的访问

加的中介者可以实现对数据的预处理、拒绝访问等等操作。。这也是目的对象保持单一指责原则的体现。

之前在编写 app 中 api 模块的时候，我使用了一个请求的代理，代码如下：

```
function errCatch (err) {
    //错误处理
}

function proxy (method = 'get', ...otherParams) {
    return new Promise(resolve => {
        axios[method].apply(axios, otherParams).then(res => {
            resolve(res)
        }).catch(errCatch)
    })
}

const $get = (URL, data = {}) => {
    let {url , queryData} = parseURL(URL)
    queryData = Object.assign(queryData, data)
    return proxy('get', url, { params: queryData })
}
```
这里使用请求代理的目的就是 想全局 catch 住 接口报错的情况，这里的 中介者 是 proxy 方法，通过这个方法发送请求，真实对象就是 axios ，中介者可以对 axios 的返回做一个处理， 确保操作者 只能得到 resolve 状态的 Promise。

#### 发布-订阅模式

> 定义了对象间一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知

js 中的事件模型其实就是使用的这个模式，所以这个模式在 js 中会经常看到。下面是一个自定义的实现方式：
<p data-height="265" data-theme-id="0" data-slug-hash="pQLxzz" data-default-tab="js,result" data-user="Xixi20160512" data-pen-title="event-model" class="codepen">See the Pen <a href="https://codepen.io/Xixi20160512/pen/pQLxzz/">event-model</a> by ruanbo (<a href="https://codepen.io/Xixi20160512">@Xixi20160512</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

这里的 Event 对象就是充当了一个全局共享的单例，可以通过这个中介者，原本没有任何联系的对象可以互相通行。vue中使用某个实例作为``eventBus``的操作基本类似。

#### 装饰者模式

> 一种动态为对象增加职责的模式

js 作为一门动态语言，使用装饰者模式很简单，示例代码如下：
```
var plane = {
    fire: function () {
        console.log('发射子弹')
    }
}

var atomDecorator = function () {
    console.log('发射原子弹')
}

var _fire1 = plane.fire

plane.fire = function () {
    _fire1()
    atomDecorator()
}
```
还有另外一种AOP（面向切面）的编程模式，在很多地方会用到，这种模式把某一函数处理过程当做一个切面，可以动态的在函数处理过程之前或之后加入其他的操作切面，整个处理过程就像是一个管道一样，数据在管道中流转，这一模式符合``开闭``原则和``单一职责``原则。

下面是这一模式的简单实现：
<p data-height="265" data-theme-id="0" data-slug-hash="NEYOoG" data-default-tab="js,result" data-user="Xixi20160512" data-pen-title="AOP" class="codepen">See the Pen <a href="https://codepen.io/Xixi20160512/pen/NEYOoG/">AOP</a> by ruanbo (<a href="https://codepen.io/Xixi20160512">@Xixi20160512</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>