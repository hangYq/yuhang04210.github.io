---
title: ES6中promise的用法
date: 2019-01-14 10:30:20
categories: 
- js
tags:
---

### promise的基本使用



#### 一、promise的含义：

* `promise`是异步编程的一种解决方案，比传统的回调函数和事件更加强大、合理。
* `promise`就是一个容器，里面保存着某个未来才会结束的事件的结果。




### 二、promise对象的特点：
* 对象的状态不受外界的影响。`promise`对象代表一个异步操作，有三种状态:`pending(进行中)`,`fulfilled(已成功)`,`reject(已失败)`。只有异步操作的结果可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。
* 状态一旦改变，就不会再变回来。`promise`对象状态的改变只能有两种情况：从`pending`到`reject`和从`pendin`g到`fulfilled`。




### 三、promise的用法：
1. `promise`对象是一个构造函数，用来生成`promise`实例。

```js
    const promise = new Promise(function (resolve,reject) {
        if(/*success*/) { // 当promise对象从pending到resolved后执行
            resolve(data);
        }else { // 当promise对象从pending到rejected后执行
            reject(data);
        }
    })

    promise.then(data => {
        console.log('data',data);
    }，err => {
        throw new Error(err);
    }) 
```


2. 模拟`promise`异步过程：得到一个随机数，如果这个随机数大于0.8则执行`resolved`，否则执行`rejected`。

```js
    function getRandom() {
        let random = Math.random();
        return new Promise((resolve,reject) => {
            if(random <= 0.8) {
                setTimeout(resolve,500,random)
            }else {
                setTimeout(reject,500,random);
            }
        })
    }

    getRandom().then(res => {
        console.log(`成功了,${res}`);
    },err => {
        console.log(`失败了,${err}`);
    })
```

3. `promise`新建后就会执行

```js
    let promise = new Promise((resolve,rejcet) => {
        console.log('promise');
        resolve();
    })


    promise.then(res => {
        console.log('resolved');
    })

    console.log('hello');
    // promise
    // hello
    // resolved
```


`promise`在新建后会立即执行，所以首先输出的是promise,然后输出hello,最后在本脚本所有的同步任务执行完成后，才会执行`then`方法指定的回调函数，所以resolved最后输出。

4. `promise`对象中，如果调用`resolve`和`reject`函数时带有参数，那么他们的参数可以传递给`then`的回调函数，`resolve`和`reject`函数的参数除了可以正常值以外，还可以是另外一个promise实例。

```js
    let promise_1 = new Promise((resolve,reject) => {
        // 模拟1s后从pending到resolved
        setTimeout(resolve,1000,'success');
    });


    // 模拟一个promise对象接受另外一个promise成功的状态
    let promise_2 = new Promise((resolve,reject) => {
        resolve(promise_1);
    })

    promise_2.then(res => {
        console.log(res);
    })
```


```js
    let promise_3 = new Promise((resolve, reject) => {
        // 模拟1s后从 pending到rejected
        setTimeout(reject,1000,'failed');
    });

    // 模拟一个promise对象接受另外一个promise失败的状态
    let promise_4 = new Promise((resolve,reject) => {
        resolve(promise_3);
    })

    promise_4.then(res => {
        console.log(res);
    },err => {
        console.log(err);
    })
```

上面代码promise_2中的`resolve`接收promise_1对象作为参数，此时，1s后promise_1执行成功，并且把promise_1的执行结果传递给Promise_2;promise_4中的`resolve`接收promise_3对象作为参数，此时，1s后promise_1执行失败，并且把promise_1的执行结果传递给Promise_4;


### 四、promise.prototype.then()

* promise实例具有`then`方法，也就是说，then方法是定义在原型对象上Promise.prototype上的，它的作用就是为promise实例状态改变时添加回调函数；
* then方法返回的是一个心得promise实例，因此可以采用链式写法，即then方法后面可以在调用另外一个then方法。

```js
    let promise_1 = new Promise((resolve,reject) => {
        resolve('success');
    })

    promise_1.then(res => {
        return res;
    }).then(res => {
        console.log(res); // success
    })
```
上面的代码，使用then方法，依次指定了两个回调函数。第一个then中的回调函数完成后，会将返回的结果作为参数，传入第二个then中的回调函数。第二个回调函数会等待第一个回调函数完成后才会执行。


### 五、promise.prototype.catch()
`promise.prototype.catch()`方法是.then(null,err=> {})或者.then(undefined,err=>{})的别名，用于指定错误发生时的回调函数。


```js
    //  推荐写法
    let promise_1 = new Promise((resolve,reject) => {
        reject('error');
    })

    promise_1.then(res => {
        return res;
    }).catch(err => {
        console.log(err);
    })


    // 等同于
    let promise_1 = new Promise((resolve, reject) => {
        reject('error');
    })

    promise_1.then(res => {
        return res;
    },err => {
        console.log(err);
    })

    // 等同于
    let promise_1 = new Promise((resolve, reject) => {
        reject('error');
    })

    promise_1.then(res => {
        return res;
    }).then(null,err => {
        console.log(err);
    })
```


### 六、promise.prototype.finally()

finally方法用于指定不管promise最后的状态是成功还是失败都会执行的操作。finally方法不接收任何参数，finally里面的操作应该是与状态无关的。

```js
    let promise = new Promise((resolve,reject) => {
        resolve('success');
    })

    promise.then(res => {
        return res;
    }).finally(() => {
        console.log('finally');
    })
```

### 七、promise.all()

`promise.all()`方法用于将多个promise实例，包装成一个新的promise实例。

```js
    let p = Promise.all([p1,p2,p3]);
```

新的p实例的状态由p1,p2,p3的状态决定：

* 只有p1,p2,p3的状态都为fulfilled，p的状态才会变为fulfilled，此时p1,p2,p3的返回值组成一个数组，传递给p的回调函数；
* 只要p1,p2,p3的状态有一个为rejected，p的状态才会变为rejected，此时第一个被rejected的实例的返回值，会传递给p的回调函数；

```js
    // 三个都为fulfilled的情况  
    let promise = [1,2,3].map(item => {
        return new Promise((resolve,reject) => {
            resolve(item);
        })
    })

    Promise.all(promise).then(res => {
        console.log('success',res);   // success [ 1, 2, 3 ]
    }).catch(err => {
        console.log('err',err);
    })   


    //  有一个为rejected的情况
    let promise = [1, 2, 3].map(item => {
        return new Promise((resolve, reject) => {
            if(item == 2) {
                reject(item);
            }else {
                resolve(item);
            }
            
        })
    })

    Promise.all(promise).then(res => {
        console.log('success',res);
    }).catch(err => {
        console.log('err',err); //  err 2
    })   

```

### 八、promise.race()
`promise.race()`方法同样将多个promise实例，包装成一个新的promise实例。

```js
    let p = Promise.race([p1,p2,p3]);
```

新的p实例的状态由p1,p2,p3的状态决定：
*  只要p1,p2,p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的promise的实例返回的值，就传递给p的回调函数。


```js
    let promise = [1, 2, 3].map(item => {
        return new Promise((resolve, reject) => {
            if(item == 2) {
                reject(item);
            }else {
                resolve(item);
            }
            
        })
    })

    Promise.race(promise).then(res => {
        console.log('success', res);  // success 1
    }).catch(err => {
        console.log('err',err);
    })
```















