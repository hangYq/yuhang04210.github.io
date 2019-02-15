---
layout: w
title: vue双向数据绑定简单实现
date: 2019-02-15 10:35:26
categories: 
- js
tags:
---


### vue双向数据绑定简单实现


#### 一、Object.defineProperty()

Object.defineProperty()允许通过属性描述对象，定义或修改属性，然后返回修改后的对象。


用法：
```js
    Object.defineProperty(object, propertyName, attributesObject)
```

Object.defineProperty方法接受三个参数，依次如下。

- 属性所在的对象
- 属性名（它应该是一个字符串）
- 属性描述对象


更过细节可以参考[Object.defineProperty()](http://javascript.ruanyifeng.com/stdlib/attributes.html#toc2)

```js
    var obj = Object.defineProperty({},"name",{
        get : function() {
            return 'get';
        },
        set : function(newValue) {
            return newValue;
        }
    })

    obj.name; // get
    obj.name = 'newName' // newName
```


#### 二、 实现简易版双向数据绑定


使用Object.defineProperty()来定义属性的set函数，属性被赋值的时候，修改Input的value值以及span中的innerHTML；然后监听input的keyup事件，修改对象的属性值，即可实现这样的一个简单的数据双向绑定。

```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <input type="text" id="input">
        输入的内容为：<span id="text-span"></span>

        <script>
            let input = document.getElementById('input');
            let textSpan = document.getElementById('text-span');


            let obj = {};
            Object.defineProperty(obj,"inputText",{
                set : function(newValue) {
                    input.value = newValue;
                    textSpan.innerHTML  = newValue;
                }
            });


            input.addEventListener('keyup',function (e) {
                obj.inputText = e.target.value;
            })
        </script>
    </body>
    </html>
```

如图：

![实现简易版双向数据绑定](/images/双向数据绑定.png) 
