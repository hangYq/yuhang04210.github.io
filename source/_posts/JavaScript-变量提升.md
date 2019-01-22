---
title: JavaScript 变量提升
date: 2019-01-22 09:30:38
categories: 
- js
tags:
---


### JavaScript 变量提升


#### 一、概念：

JavaScript中函数以及变量的声明都会提升到函数的最顶部。变量可以在使用后声明，也就是说变量可以先使用在声明。

变量提升：函数声明和变量声明总是会被解释器悄悄地被"提升"到方法体的最顶部。

#### 二、javaScript中只有声明的变量会提升，初始化的值不会

如下代码：

```js
    var a = 10;
    console.log(a + b); // NaN
    var b = 10;
```

这是因为变量声明 (var b) 提升了，但是初始化(b = 10) 并不会提升，所以 a + b 为NaN;


#### 三、JavaScript简单解析机制

浏览器JavaScript引擎遇到`script`标签就会对js进行预解析，将变量`var` 和`function`声明提升，但是不会执行function，然后就进入上下文执行，上下文执行还是执行预解析同样的操作，直到没有`var`和`function`，就开始执行上下文。



如：

```js
    a = 1;

    f();

    var a;

    function f() { conole.log('0000')};
```

预解析：
```js
    function f() { console.log('0000)};

    var a ;

    a = 1;

    f();
```

上面除了函数声明方式以外还可以用匿名函数的方式。

如：

```js
    var f = function () {};

    f();
```

注意：

    1. 函数声明提升直接把整个函数提升到执行环境的最顶端，所以上面的f()提升后是function f() {};
    2. 函数优先，虽然函数声明和变量声明都会被提升，但是函数会首先被提升，然后才是变量;
    3. 使用匿名函数的方式不存在函数提升，因为函数声明使用的是变量表示的，所以匿名函数不存在函数提升，只存在变量提升。

如：

```js

    var f = function () {
        console.log(2);
    }

    function f() {
        console.log(1);
    }

    f(); // 2
```


预解析:

```js
    function f() {
        console.log(1);
    }

    var f;
    
    f = function () {
        console.log(2);
    }

    f()
```
这里的输出结果是2


还有另外一种情况，如：

```js 
    f(); // 1

    function f() {
        console.log(1);
    }

    var f = function () {
        console.log(2);
    }

```




所以这里的输出为1，因为这里的函数表达式（匿名函数）不会被函数提升，就像上面的预解析一样，f虽然也被提升了，但是他是当做了一个变量，因为这个时候f是通过`var f`声明的变量，只不过他指向了一个函数，所以他提升后的代码等价于下面的预解析代码：

预解析：

```js
    function f() {
        console.log(1);
    }

    var f;

    f();

    f = function () {
        console.log(2);
    }
```


#### 四、let和const

var命令会发生”变量提升“现象，即变量可以在声明之前使用，值为undefined。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。

为了纠正这种现象，let、const命令改变了语法行为，let、const声明的变量只在它所在的代码块有效，它所声明的变量一定要在声明后使用，否则报错。

```js
    // ar 的情况
    console.log(foo); // 输出undefined
    var foo = 2;

    // let 的情况
    console.log(bar); // 报错ReferenceError
    let bar = 2;
```

























