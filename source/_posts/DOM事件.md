---
title: 'DOM事件'
date: 2019-01-04 14:22:35
categories: 
- js
tags:
---



#### DOM事件类

##### 一、基本概念：DOM事件级别

|DOM事件级别| DOM事件|描述|
|:--------:|:------:|:----:|
|DOM0|element.onclick=function(){}|DOM0时代制定的DOM事件标准|
|DOM2|element.addEventListener('click',function(){ },false)|①DOM2时代制定的事件监听；②DOM1没有制定与事件相关的；③false,默认表示冒泡阶段触发，true表示捕获阶段|
|DOM3|element.addEventListener('keyup',function(){ },false)|DOM3在DOM2的基础上增加了一些鼠标键盘事件|


##### 二、DOM事件模型（冒泡、捕获）
>- 事件冒泡：是由IE团队提出来的，即事件是由最具体的那个元素j接收，然后逐级向上传播

>- 事件捕获： 事件捕获是由NetSpace团队提出来的，是由最上一级节点先接收事件，然后向下传播到具体的节点

##### 三、DOM事件流
"DOM2级事件"规定了事件l流包含3个阶段，事件捕获阶段，目标阶段和事件冒泡阶段。首先发生的事件捕获阶段，然后实际的目标接收到事件，最后阶段是冒泡阶段。


##### 四、DOM事件捕获的具体流程
window  -> document(文档对象) -> html -> body -> div(具体目标元素)

备注：具体代码参考[DOM事件捕获流程](https://github.com/yuhang04210/full-stack-knowledge-note/blob/master/DOM%E4%BA%8B%E4%BB%B6/DOM%E4%BA%8B%E4%BB%B6%E6%8D%95%E8%8E%B7%E6%B5%81%E7%A8%8B.html)


##### 五、DOM事件冒泡的具体流程
div(具体目标元素) -> body -> html -> document(文档对象) -> window 

备注：具体代码参考[DOM事件冒泡流程](https://github.com/yuhang04210/full-stack-knowledge-note/blob/master/DOM%E4%BA%8B%E4%BB%B6/DOM%E4%BA%8B%E4%BB%B6%E5%86%92%E6%B3%A1%E6%B5%81%E7%A8%8B.html)


##### 六、Event对象的具体应用

|事件对象|描述|
|:-----:|:---:|
|event.preventDefault()|阻止默认事件|
|event.stopPropagation()|阻止冒泡|
|event.currentTarget()|当前绑定事件对象的元素|
|event.target()|当前被点击的元素|

##### 七、自定义事件
> 自定义不带参数事件
```javascript
    let ev = new Event('cat');
    document.addEventListener('cat',function() {
        console.log('自定义不带参数事件');
    })
    document.dispatchEvent(ev);
```

> 自定义可以传参事件
```javascript
    let ev = new CustomEvent('dog',{ detail: {
        a : '参数a'
    }});
    document.addEventListener('dog',function(e) {
        console.log('自定义事件参数为',e.detail);
    })
    document.dispatchEvent(ev);
```
备注：具体代码参考[自定义事件](https://github.com/yuhang04210/full-stack-knowledge-note/blob/master/DOM%E4%BA%8B%E4%BB%B6/%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BA%8B%E4%BB%B6.html)


