---
title: JavaScript面试题总结(二)
date: 2019-02-22 21:49:57
categories: 
- js
tags:
---


### JavaScript面试题总结(二)


#### 一、题目一：


#### 描述： 

    1. 获取某个页面所使用的所有标签
    2. 并且统计出现次数最多的标签



1. 第一步先获取所有的标签，并将类数组对象转化为数组，统计每一种标签出现的次数

```js
/**
 * 获取所有的标签节点，并且对每一种标签进行统计个数，返回一个对象
 */
function getAllTags () {
    let nodeList = document.querySelectorAll("*");

    obj = Array.from(nodeList).reduce((pre,next) => {
        pre[next.localName] = pre[next.localName] ? ++pre[next.localName] : 1;
        return pre;
    },{});
    return obj;
}
```

#### 注意：

    1. document.querySelectorAll("*")可以获取所有的节点对象，这里的返回结果为类数组对象;
    2. Array.from()可以把一个类数组对象转化为一个数组；
    3. pre[next.localName] = pre[next.localName] ? ++pre，这里要注意的是，返回的类数组对象的每一项也都是一个对象，相同的节点有不同的类名也会归为一个不同项，我们可以通过判断localName来判断是否是相同的标签。

如图：

![类数组节点图](/images/nodeList.png)



2. 统计出现次数最多的标签

```js
function getMaxCount(obj) {
    let result = {
        count: 0,
        tag : ''
    };

    Object.keys(obj).forEach(item => {
        if (result.count < obj[item]) {
            result.count = obj[item];
            result.tag = item;
        }
    })

    return result;
}
```

```js
function getResult () {
    let obj = getAllTags();

    let result = getMaxCount(obj);
    return result;
}
```




上面的代码虽然解决此题，但是还有一个问题，如果有两个或者多个标签出现的次数相同并且次数最多，上面的代码并不能获取正确的结果，我们仍然需要进行改进,我们可以把getMaxCount方法进行改造。




我们可以在原有基础上，把result对象tag改变为tag数组，用来存放所有的标签，用tempTags来临时存放出现次数相同的标签。如果`result.count < obj[item]`,需要注意的是，要把tempTags临时数组清空，因为此时已经有出现次数更多的标签出现。

```js
/**
 * 
 * @param {object} obj 要传入的类数组对象
 * 返回一个对象
 */
function getMaxCount (obj) {
    // 临时存储个数相同的标签
    tempTags = [];

    /**
     * count : 标签个数
     * tags : 出现次数相同的标签
     */
    let result = {
        count : 0,
        tags : []
    };

    Object.keys(obj).forEach( item => {
        if(result.count < obj[item]) {
            result.count = obj[item];
            result.tags[0] = item;
            // 如果有出现次数更多的标签，把临时数组置为空
            tempTags = [];
        } else if (result.count === obj[item]) {
            tempTags.push(item);
        }
    })


    if(tempTags.length > 0) {
        tempTags.forEach(item => {
            result.tags.push(item);
        })
    }

    return result;
}
```


如图：


![输出结果图](/images/nodeList1.png)




#### 二、题目二：

#### 描述： 

有A、B、C三个请求，C依赖于A和B连个请求，如何实现？

```js
let getA = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(2);
    }, 3000);
})


let getB = new Promise((reolve, reject) => {
    setTimeout(() => {
        reolve(3);
    }, 1000);
})


function getC(a, b) {
    return a + b;
}


Promise.all([getA, getB]).then(data => {
    console.log(data);
    return getC(data[0],data[1]);
}).then(res => {
    console.log('res',res); // 5
})
```

这里可以使用promise.all()来并行实现，用来提高效率。当然也可以使用async来实现，不过async是同步来执行的，可能会有一定的效率问题。

#### 本题参考资料：

[1. es6中的promise.all使用问题](https://segmentfault.com/q/1010000008174264)

[2. ES6 Promise 并行执行和顺序执行](https://www.jianshu.com/p/dbda3053da20)

[3. 阮一峰es6中promise.all的使用](http://es6.ruanyifeng.com/#docs/promise#Promise-all)






