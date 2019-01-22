---
title: JavaScript面试题总结
date: 2019-01-22 11:38:06
categories: 
- js
tags:
---


### JavaScript面试题总结


#### 题目一：


```js
    var a = 1;
    
    if(!(b in window)) { 
        var b = 2;
        a += 1;
    }else {
        a += 2;
    }
    console.log(a) // 3 
    console.log(b) // undefined
```

上面的代码可以理解为：在if条件中，变量提升，可以把b提升到作用域顶部，又因为undefined是window的一个全局变量，所以`b in window`返回true，执行结果为3 undefined


```js
    var a = 1;
    var b;

    if(!(b in window)) { // undefined in window 返回true，因为undefined是window的一个全局变量
        b = 2;
        a += 1;
    }else {
        a += 2;
    }
    console.log(a)
    console.log(b)
```


#### 题目二：

```js
    var m = 1;

    function log() {
        var m = 2;
        return function () { // 闭包
            m += 1;
        }
    }

    var _log = log(); // function () { m += 1 } 这里的m是log函数里的局部变量m=2

    _log(); // 把log函数里的m+1，此时局部变量m 为3

    console.log(m);		// 这里的m为全局变量m ,所以为1
```

#### 题目三：

代码一：

```js
    var array = [1,2,3];

    function test (arr) {
        arr = [];
    }

    test(array);

    console.log(array); // [1,2,3];
```

很多人会以为此题的答案是`[]`,而实际上是 `[1,2,3]`,其实我们只需要明白一点，函数传参是`按值传递`而不是`按引用传递`的。



js传递参数: 

    js中所有函数参数的传递都是按值传递的，也就是说，函数外部的值复制给函数内部的参数，就和把一个变量的值复制到另外一个变量一样。基本类型值的传递如同基本类型变量的复制一样，而引用类型值的传递，则如同应用类型变量的复制一样。


上面的代码改成如下，代码二：

```js
    var array = [1,2,3];

    function test(arr) {
        arr.push(4);
    }


    test(array);
    console.log(array);    [1, 2, 3, 4]
```

代码一中的是把外部变量array复制一份传递给参数arr,然后在函数内部，又重新赋值为一个空数组，这里切断了他与外部变量array的联系，也就是改变了指针的执向，而外部array还是执向原来堆内存中的数组对象，这时堆内存里相当于有两个数组对象。代码二中，它们执向的是同一个堆内存中的数组对象，所以在函数内部改变数组，会影响到外部。


#### 题目四：

```js
    // 1. 任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数的外部访问这些变量。
    // 2. 私有变量包括函数的参数、局部变量和在函数内部定义的其他函数
    function Foo () {
        function getName() { // 在函数内部定义了一个函数（Foo的私有变量）
            console.log(1);
        }

        return this;
    }

    // 在函数Foo上定义了一个静态方法
    Foo.getName = function() {
        console.log(2);
    }

    // 在Foo的原型对象上定义了一个共享方法
    Foo.prototype.getName = function () {
        console.log(3);
    }


    // 函数表达式
    var getName = function () {
        console.log(4);
    }


    // 函数声明
    function getName () {
        console.log(5);
    }


    Foo.getName();		// 2
    getName();			// 4
    Foo().getName();	// 4
    getName();          // 4
```


上面的代码可以理解为下面这样：
```js
    function Foo () {
        function getName() { // 在函数内部定义了一个函数（Foo的私有变量）
            console.log(1);
        }

        return this;
    }


    // 函数声明(函数提升)
    function getName () {
        console.log(5);
    }

    // 变量提升
    var getName;

    // 在函数Foo上定义了一个静态方法,函数的静态静态方法和静态属性只能通过构造函数来访问，实例访问不到
    Foo.getName = function() {
        console.log(2);
    }

    // 在Foo的原型对象上定义了一个共享方法
    Foo.prototype.getName = function () {
        console.log(3);
    }


    // 函数表达式
    getName = function () {
        console.log(4);
    }

    Foo.getName();		// 2
    getName();			// 4
    Foo().getName();	// 4
    getName();			// 4
```


在上面的题目中，主要考察了js 的function为什么可以添加属性,类的静态方法，变量提升，原型对象，理解这几个知识点就可以了。

1. js 的function为什么可以添加属性?

```js
    // 1.构造函数的是公有属性
    function person(){
        this.name = 'Tom';
    }
    
    // 构造函数的静态属性
    function person(){

    }
    person.name = 'Tom';
    
    // 构造函数的原型共享属性
    function person(){

    }
    person.prototype.name = 'Tom'
```

解释：

①因为在js里函数也是对象，是`Function`的实例，`function person(){ }; person.name = 'Tom';`这是函数对象上直接定义了name属性，只能通过函数名访问，不能通过实例对象访问；
②因为Js里面函数也是对象，函数其实也有另外一种写法，`var message = new Function('msg','alert(msg)');
// 等价于：function message(msg) { alert(msg); }`;函数也是new出来的，所以函数其实也是一个对象。所以对象可以添加属性。




2. 类的静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
    class Foo {
        static classMethod() {
            return 'hello';
        }
    }

    Foo.classMethod() // 'hello'

    var foo = new Foo();
    foo.classMethod() // foo.classMethod is not a function
```

上面代码中，Foo类的classMethod方法前有static关键字，表明该方法是一个静态方法，可以直接在Foo类上调用（Foo.classMethod()），而不是在Foo类的实例上调用。如果在实例上调用静态方法，会抛出一个错误，表示不存在该方法。


3. 变量提升(参考JavaScript 变量提升):
[JavaScript 变量提升](https://yuhang04210.github.io/2019/01/22/JavaScript-%E5%8F%98%E9%87%8F%E6%8F%90%E5%8D%87/)



4. 原型对象(参考js对象和原型链)
[js对象和原型链](https://yuhang04210.github.io/2019/01/06/js%E5%AF%B9%E8%B1%A1%E5%92%8C%E5%8E%9F%E5%9E%8B%E9%93%BE/#more)



参考文献：
1. [js 的function为什么可以添加属性](https://www.cnblogs.com/hanguidong/p/9296647.html)
2. [阮一峰老师es6教程](http://es6.ruanyifeng.com/#docs/class#%E9%9D%99%E6%80%81%E5%B1%9E%E6%80%A7)
3. 《JavaScript高级程序设计》第四章 4.1.3 传递参数







