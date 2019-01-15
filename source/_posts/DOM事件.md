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

```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>DOM事件捕获流程</title>
    </head>
    <body>
        <div>
            <button id="btn">按钮</button>
        </div>
        <script>
            // 所有的addEventListener最后一个参数是true，表示在捕获阶段，不加默认为false，在冒泡阶段触发
            // window 
            window.addEventListener('click',function() {
                console.log('window')            
            },true)
            // document
            document.addEventListener('click',function () {
                console.log('document')
            },true)
            // html
            document.documentElement.addEventListener('click',function(){
                console.log('html')
            },true)
            // body
            document.body.addEventListener('click',function(){
                console.log('body')
            },true)
            // div
            let btn = document.getElementById("btn");
            btn.addEventListener('click',function () {
                console.log('div')
            },true)
        </script>
    </body>
    </html>
```

##### 五、DOM事件冒泡的具体流程
div(具体目标元素) -> body -> html -> document(文档对象) -> window 


```html
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>DOM事件冒泡流程</title>
    </head>

    <body>
        <div>
            <button id="btn">按钮</button>
        </div>
        <script>
            // window 
            window.addEventListener('click', function () {
                console.log('window')
            }, false)
            // document
            document.addEventListener('click', function () {
                console.log('document')
            }, false)
            // html
            document.documentElement.addEventListener('click', function () {
                console.log('html')
            }, false)
            // body
            document.body.addEventListener('click', function () {
                console.log('body')
            }, false)
            // div
            let btn = document.getElementById("btn");
            btn.addEventListener('click', function () {
                console.log('div')
            }, false)
        </script>
    </body>

    </html>
```


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



```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>自定义事件</title>
    </head>
    <body>
        <div>
            <button id="btn">按钮1</button>
            <button id="btn2">按钮2</button>
        </div>
        <script>
            {
                // 一、 自定义一个不可传参事件
                // new 一个custon事件对象，事件名为custom
                let event = new Event('custom');
                // 监听自定义事件
                document.addEventListener('custom', function () {
                    console.log('自定义事件')
                })
                let btn = document.getElementById('btn');
                btn.addEventListener('click', function () {
                    console.log('click事件');
                    // 触发自定义事件
                    document.dispatchEvent(event);
                })
            }
        </script>


        <script>
            {
                // 自定义一个传参事件
                // new 一个自定义事件对象，并且传递参数
                /*
                    * bubbles : 是否冒泡
                    * cancelable ： 是否可取消该事件
                    * detail : 需要传递的事件参数
                */
                let event_2 = new CustomEvent('customEvent', 
                    { bubbles: 'true', cancelable: 'true', detail : { a: '参数a', b: '参数b' }} 
                );
                // 监听自定义事件
                document.addEventListener('customEvent', function (e) {
                    console.log('自定义事件,参数为:', e);
                })
                let btn_2 = document.getElementById("btn2");
                btn_2.addEventListener('click',function(){
                    console.log('clcik事件2');
                    document.dispatchEvent(event_2);
                })
                
            }
        </script>
    </body>
    </html>
```

