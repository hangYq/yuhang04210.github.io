---
title: ES6中class的用法
date: 2019-01-14 10:14:04
categories: 
- js
tags:
---

### class的基本使用

#### 一、es6的 class 与es5 的构造函数

```js
    //  es6 class
    class Person {
        constructor(name,age) {
            this.name = name;
            this.age = age;
        }

        getName () {
            console.log('get name',this.name);
        }
    }

    // es5 构造函数
    function Person (name,age) {
        this.name = name;
        this.age = age;
    }

    Person.prototype.getName = function () {
        console.log('get name',this.name);
    }
```

* `class` 里面的`constructor`方法就是构造方法，而this关键字则代表*实例对象*;
* 也就是说`es5`的构造函数`Person`，对应的就是`es6`的`Person`类的`constructor`方法。


#### 二、class的数据类型与constructor的指向

```js
    class Person {
        constructor(name,age) {
            this.name = name;
            this.age = age;
        }

        getName () {
            console.log('get name',this.name);
        }
    }
    typeof Person // "function"
    Person.prototype.constructor === Person // true
```

* `typeof Person` 打印结果是"function",表名类的数据类型就是*函数*;
* `Person.prototype.constructor === Person`打印结构是true,表名类本身就指向*构造函数*。


#### 三、构造函数的prototype属性，在es6中继续存在，实际上es6中class的所有方法都是定义在类的prototype属性上的

```js
    class Person {
        constructor (name,age) {
            this.name = name;
            this.age = age;
        }

        getName () {
            console.log('get name',this.name);
        }
    }

    // 等同于
    Person.prototype = {
        constructor() {},
        getName() {},
    }
```


#### 四、constructor方法

`constructor`方法是类默认的构造方法，通过`new`命令生成实例对象，自动调用该方法，一个类必须有一个`constructor`方法，如果没有显示定义，则会默认添加。

```js
    class Person  {

    }

    // 等同于
    class Person {
        constructor(){}
    }
```
`constructor`方法默认返回实例对象(即`this`)，完全可以指定返回另外一个对象。

```js
    class Person {
        constructor() {
            return Object.create(null);
        }
    }
```

#### 五、this指向

* 类的方法内部如果含有this，则默认指向类的实例;
* 如果单独使用，很可能报错;


```js
    class Person {
        constructor(name, age) {
            this.name = name;
            this.age = age;
        }

        getName() {
            console.log('get name');
            this.getAge();
        }

        getAge() {
            console.log('get age',this.age);
        }
    }


    let p = new Person('jack',20);
    p.getName(); 

    const { getName } = p;
    getName(); // TypeError: Cannot read property 'getAge' of undefined
```


* getName 方法中的this默认指向类的实例p;
* 如果`const { getName } = p`,将这个方法单独提取出来，this会指向当前指向环境，而不是当前实例对象,会因为找不到`getAge()`方法而报错;



##### 解决方案一：在构造方法中绑定this

```js
    class Person {
        constructor(name, age) {
            this.name = name;
            this.age = age;
            // 在constructor方法中绑定this
            this.getName = this.getName.bind(this);
        }

        getName() {
            console.log('get name');
            this.getAge();
        }

        getAge() {
            console.log('get age', this.age);
        }
    }


    let p = new Person('jack', 20);
    p.getName();

    const { getName } = p;
    getName();
```

##### 解决方案二：箭头函数

```js
    class Person {
        constructor(name, age) {
            this.name = name;
            this.age = age;
        }

        getName = () =>  {
            console.log('get name');
            this.getAge();
        }

        getAge() {
            console.log('get age', this.age);
        }
    }


    let p = new Person('jack', 20);
    p.getName();

    const { getName } = p;
    getName();
```
#### 六、类的静态方法

    类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前面加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为静态方法。

```js
    class Person {
        static getName() {
            console.log('hello');
        }
    }
    Person.getName(); // hello


    let p = new Person();
    p.getName(); // p.getName is not a function
```
* Person类的getName方法前面有一个`static`关键字，表明该方法是一个静态方法;
* 该方法只能在Person类来调用，不能在实例上调用;


```js
    class Person {
        static getName() {
            console.log('hello');
            // 静态方法里的this代表当前类，可以调用其他静态方法
            this.getAge();
        }

        static getAge() {
            console.log('age');
        }
    }
    Person.getName(); // hello age
```
* 如果静态方法里面包含`this`关键字，则this指向的是类，而不是实例。

父类的静态方法可以被继承：

```js
    class Person {
        static getName() {
            console.log('parent hello');
        }
    }

    class Child extends Person {

    }

    Child.getName(); // parent hello
```
* 子类可以继承父类的静态方法，如上面代码，子类`Child`继承父类`Person`的`getName`静态方法，并且调用;




```js
    class Person {
        static getName() {
            console.log('parent hello');
        }
    }

    class Child extends Person {
        static  getChildName() {
            // 通过super对象调用父类的静态方法
            super.getName();
        }
    }

    Child.getChildName(); // parent hello
```
* 静态方法也可以通过`super`对象来调用;


#### 七、类的静态属性

    静态属性指的是class本身的属性，即class.PropName,而不是定义在实例对象上的属性。

方式一：老写法

```js
    class Person {

    }
    Person.prop = 'tom';
    Person.prop;  // tom
```

方式二：新写法

```js
    class Person {
        static prop = 'tom';
    }
    Person.prop;
```


#### 八、实例属性新写法：

实例属性除了可以写在`constructor`方法里面，还可以写在类的*最顶层*。

```js
    // 老写法
    class Person {
        constructor (age) {
            this.age = 20
        }
    }

    // 新写法
    class Person {
        age = 20;
    }
```


#### 九、extends继承的基本用法

```js
    class Person {
        
    }
    class Child extends Person  {

    }
```
* Child 通过`extends`继承了父类Person的所有属性和方法;


```js
    class Person {

    }
    class Child extends Person  {
        constructor() {

        }
    }

    let c = new Child(); // Must call super constructor in derived class before accessing 'this' or returning from derived constructor
```

* 子类必须在`constructor`方法中调用`super`方法，否则会报错；
* es6的继承机制是先将父类的实例对象的属性和方法，加到this上(所以必须先调用super方法)；然后在再子类的构造函数修改this；



```js
    class Person {

    }
    class Child extends Person  {
        
    }

    // 等同于
    class Person {

    }
    class Child extends Person  {
        constructor(...args) {
            super(...args)
        }
    }
```
* 如果子类没有`constructor`方法，这个方法会被默认添加，并且同时会在构造函数中调用super方法，也就是说，不管有没有显示定义，任何一个子类都有`constructor`方法。



```js
    class Person {
        constructor(name,age) {
            this.name = name;
            this.age = age;
        }
    }
    class Child extends Person  {
        constructor(name,age,gender) {
            // this.gender = gender; // Must call super constructor in derived class before accessing 'this' or returning from derived constructor
            super(name,age);
            this.gender = gender; // 正确
        }
    }


    let c = new Child('tom',20,'男');
```
* 注意：只有在子类的构造函数中，只有调用`super`之后，才可以使用`this`关键字；
* 上面的代码中，由于在子类的`constructor`方法中没有调用`super`关键字，就使用`this`关键字会报错，而放在`super`之后就是正确的。


#### 十、super关键字

super关键字既可以当作函数使用，也可以当作对象使用。

*  `super`作为函数调用，代表*父类的构造函数*，es6要求，子类的构造函数必须执行一次`super`函数。`super`虽然代表父类的构造函数，但是返回的却是子类的实例，即`super`内部的`thi`s执向的是子类的实例。作为函数时，`super()`只能用在子类的构造函数中，用在其他地方会报错。
*  `super`作为对象是，在普通方法中，代表父类的原型对象，在静态方法中，执向父类。




















