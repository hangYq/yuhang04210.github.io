---
title: js对象和instanceof底层原理以及typeof的使用
date: 2019-01-15 11:28:40
categories: 
- js
tags:
---




### js原型链和instanceof底层原理以及typeof的使用

### 一、typeof操作符

鉴于 ECMAScript 是松散类型的，因此需要有一种手段来检测给定变量的数据类型—— typeof 就是负责提供这方面信息的操作符。对一个值使用 typeof 操作符可能返回下列某个字符串。

|变量值类型|typof返回值|
|:-----:|:-------:|
|值未定义|undefined|
|布尔值|boolean|
|字符串|string|
|数值|number|
|对象或null|object|
|函数|function|

```js
    var a;
    var b = null;
    var s = '123';
    var d = true;
    var o = {};
    var f = function () {};
    var num = 123;

    console.log(typeof a); // undefined
    console.log(typeof b); // object
    console.log(typeof s); // string
    console.log(typeof d); // boolean
    console.log(typeof o); // object
    console.log(typeof f); // function
    console.log(typeof num);    // number
```



### 二、instanceof

`instanceof` 可以判断一个引用对象是否属于某构造函数，`instanceof`的判断逻辑是从当前对象引用的`__proto__`顺着原型链一层一层网上找，是否能够找到对应的`prototype`，如果找到了就返回`true`，否则返回`false`。



1. 模拟instanceof的实现


```js
    /**
     * 
     * @param {Object} L  instanceof左侧的参数
     * @param {Function} R  instanceof右侧的参数
    */

    function instance_of (L,R) {
        let r = R.prototype;    // 取R的显示原型
        let l = L.__proto__;    // 取L的隐式原型   

        while (true) { // null 不是任何对象的实例，如果左侧为null的话，直接返回false
            if(L === null) {
                return false;
            }
            if(l === r) { // 当R的显示原型和L的隐式原型相等时返回true
                return true;
            }
            
            l = l.__proto__;
        }
    }
```

2. 使用instanceof时有两种情况需要考虑：

*   第一种情况：没有发生继承时；
*   第二种情况：发生继承时；

##### 第一种情况：没有发生继承时


```js
    function Parent (gender) {
        this.gender = gender;
    }

    function Child (name,age) {
        this.name = name;
        this.age = age;
    }


    let c = new Child('tom',20);
    let p = new Parent('男');

    console.log(c instanceof Child);        // true
    console.log(p instanceof Parent);       // true
    
    console.log( c instanceof Object);      // true
    console.log( p instanceof Object);      // true

```



没有发生继承时，instanceof的工作流程分析：


```js
    function instance_of (L,R) {
        let r = R.prototype;    // r为Child.prototype
        let l = L.__proto__;    // l为c.__proto__  

        while (true) { 
            if(L === null) { // 不通过
                return false;
            }
            if(l === r) { // 判断 此时l为c.__proto__ 和r为Child.prototype是否相等
                return true;
            }
            
            l = l.__proto__;
        }
    }
```

上面的代码中执行`c instanceof Parent`的时候，l和r执向的是同一个原型对象，所以返回true



##### 第二种情况：发生继承时



```js
    function Parent (gender) {
        this.gender = gender;
    }

    function Child (name,age,gender) {
        Parent.call(this,gender);
        this.name = name;
        this.age = age;
    }


    Child.prototype = new Parent(); // 改变了原型指向，实现继承

    var c = new Child('tom',20,'男');

    console.log( c instanceof Child);   // true
    console.log( c instanceof Parent);  // true
    console.log( c instanceof Object);  // true

```
发生继承时，instanceof的工作流程分析：

① 首先看一下c instanceof Child

```js
    function instance_of (L,R) {
        let r = R.prototype;    // r为Parent的实例p
        let l = L.__proto__;    // l也为Parent的实例p  

        while (true) { 
            if(L === null) { // 不通过
                return false;
            }
            if(l === r) { // 判断 此时l为Parent的实例p 和r为Parent的实例p相等
                return true;
            }
            
            l = l.__proto__;
        }
    }
```

即使发生了原型继承，`c instanceof Child` 依然是成立的



② 再看一下c instanceof Parent

```js
    function instance_of (L,R) {
        let r = R.prototype;    // r为Parent.prototype
        let l = L.__proto__;    // l也为Parent的实例p  

        while (true) { 
            if(L === null) { // 不通过
                return false;
            }
            if(l === r) { // 判断 此时l为Parent的实例p 和r为Parent.prototypepb不相等
                return true;
            }
            
            l = l.__proto__; // 此时l为Parent.prototype
        }
    }
```


c instanceof Parent返回值为true，这就证明了c继承了Parent。


结论：

* 用于判断一个引用类型是否属于某构造函数；

* 还可以在继承关系中用来判断一个实例是否属于它的父类型。