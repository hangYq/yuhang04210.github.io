---
title: js模块化
date: 2019-02-21 13:13:19
categories: 
- js
tags:
---


### js模块化


#### 一、CommonJs

node.js的出现标志着"javaScript模块化编程"的正式诞生。因为老实说，在浏览器环境下，没有模块化并没有什么太大的影响，毕竟网页应用程序的复杂性有限。但是在服务端，一定要有模块化，与操作系统和其他应用程序互动，否则根本没法编程。Node.js是CommonJs规范的实现，webpack也是CommonJs规范的实现。


##### 1. CommonJs定义的模块分为：

    ① 模块引用：require，require用来引入外部模块（`require是同步的`）
    ② 模块定义：exports，exports对象用来导出当前模块的方法和变量，唯一的导出口
    ③ 模块表示： module，module对象就代表模块本身

#### 2. 浏览器不兼容CommonJs的原因：

浏览器不兼容CommonJs的根本原因，在与浏览器缺少四个node.js环境的变量：

1. module
2. exports
3. require
4. global

注意：
    
    只要能够提这四个变量，浏览器就可以加载CommonJs模块。



#### 二、AMD


##### 1. AMD的出现

基于`CommonJs`的`node.js`出来以后，服务端的模块概念已经形成，很自然大家就想要客户端模块。而且最好两者可以兼容，一个模块不用修改，在服务端和浏览器都可以运行。但是有一个很大的局限，使得CommonJs规范不适用于浏览器。


```js
    const math = require('math');
    math.add(1,2);
```

第二行math.add(1,2)必须在第一行require加载完成后，才可以执行，也就是说如果加载时间很长，浏览器就会在哪里等待，因为在CommonJs中require是同步的。

上面的问题对于服务端不是一个问题，因为所有的文件都存放在本地硬盘里，可以同步加载完成，等待的时间就是硬盘的读取时间。但是对于浏览器而言，是一个大的问题，因为所有的文件都是放在服务器，等待的时间取决于网络，可能需要等待的时间比较久，浏览器处于假死状态。

因此浏览器的模块不能采用同步加载，只能使用异步方式。这就是AMD诞生的背景。


CommonJs的实现是为了后端实现而制定的，它不适合前端，AND(异步模块定义)的出现就是为了前端实现而制定的规范。


AMD是"Asynchronous Module Definition"的缩写，意思就是"异步模块定义"。它采用的是异步方式加载模块，模块的加载不影响它后面的语句的执行，所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成后，才会调用这些语句。


AMD的实现也是采用require来实现的，但是不同于CommonJs，它有两个参数：


```js
require([module],callback);
```

第一个参数[module],是一个数组，就是要加载的模块；第二个参数callback，就是加载成功之后的回调函数。如果将前面的代码改成AMD的形式，就是下面这样：

```js
require(['math'],function(math) {
    math.add(1,2);
})
```


math.add()与math加载不是同步的，浏览器不会发生假死，所以很显然浏览器比较适合AMD规范。目前`require.js`就实现了AMD规范。


##### 2. 为什么用require.js?

最早的时候，所有的JavaScript代码都放在一个文件下，只要加载这一个文件就够了，后来代码越来越多，一个文件不够用了，必须分为两个文件或者更多文件，一次加载。就出现了这种形式：


```js 
<script src="a.js"></script>
<script src="b.js"></script>
<script src="c.js"></script>
<script src="d.js"></script>
```


这段代码依次加载多个js文件，不过这样的写法有很大的缺点：

    1. 加载的时候，浏览器会停止渲染；
    2. 由于js文件之间有一定的依赖性，所以要保持js的先后顺序，否则就会出错。

require.js的出现就是为了解决以上两个问题：

    1. 实现js文件的异步加载，避免网页失去响应；
    2. 管理模块之间的依赖性，便于代码的编写和维护。


#### 三、CMD

大名远扬的玉伯写了seajs，就是遵循他提出的CMD规范，与AMD蛮相近的。


Sea.js 更贴近 CommonJS Modules/1.1 和 Node Modules 规范。Sea.js则专注于web端浏览器，同时通过node扩展的方式可以很方便跑在node环境中。

require.js想成为浏览器端的模块加载器，同时也想成为Rhino / Node等环境的模块加载器。


### 四、 ES6模块化

[参考阮一峰老师的教程](http://es6.ruanyifeng.com/#docs/module)


参考文献

[1. js模块化编程之彻底弄懂CommonJS和AMD/CMD！](https://www.cnblogs.com/moxiaowohuwei/p/8692359.html)

[2. require.js与Sea.js的异同](https://github.com/seajs/seajs/issues/277)

[3. Javascript模块化编程（一）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)

[4. Javascript模块化编程（二）：AMD规范](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)

[5. Javascript模块化编程（三）：require.js的用法](http://www.ruanyifeng.com/blog/2012/11/require_js.html)

[6. 阮一峰ES6 Module的语法](http://es6.ruanyifeng.com/#docs/module)












