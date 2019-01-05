---
title: css盒模型
date: 2019-01-05 20:00:08
categories: 
- css
tags:
---


### css盒模型

#### 一、css盒模型分类：

css盒模型分为IE盒模型和标准盒模型，以下是IE盒模型和标准盒模型的对比

|分类|区别|box-sizing|
|:------:|:----:|:----:|
|IE盒模型|content不包含padding和border|border-box|
|标准盒模型|content包含padding和border|content-box|



#### 二、js如何获取到盒模型的宽度和高度？

以下是三种获取dom节点盒模型宽度和高度的方法以及他们的优缺点

|方式|优点|缺点|
|:---:|:---:|:---:|
|dom.style.width/height|略|只能获取到内联样式的宽度和高度|
|dom.current.style.width/height|可以获取到当前dom节点的宽度和高度、无论是内联还是外部链接样式|只有IE浏览器支持|
|window.getComputedStyle(dom).width/height|兼容性比较好，可以获取到当前dom节点的宽度和高度，无论是内联还是外部链接样式|略|



### 三、BFC

> BFC定义

    BFC(Block formatting context)块级格式化上下文。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

> BFC的布局规则
* 内部的box会在垂直方向上一个接一个的放置；
* BFC在页面上是一个独立的容器，不会影响到外部元素也不会被外部元素影响到；
* BFC计算高度时，浮动元素也参与在内；
* BFC区域不会与浮动元素发生重叠(通常可以用来清除浮动)；
* 属于同一个BFC的两个相邻box的margin会发生重叠；

> 如何生成BFC？
* overflow不为visible；
* float为left、right；
* position为absolute、fixed；
* display为inline-block、table、table-cell、flex等；

> BFC的应用

应用一：解决属于同一个BFC内相邻的box发生margin重叠问题


1.外边距重叠情况
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        .container {
            border: 1px solid #fff;
            width: 200px;
            height: 200px;
            background: pink;
        }
        .children_1,.children_2 {
            height: 50px;
            line-height: 50px;
            width: 50px;
            text-align: center;
            margin: 20px;
        }
        .children_1 {
            border: 1px solid red;
        }
        .children_2 {
            border: 1px solid blue;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="children_1">box1</div>
        <div class="children_2">box2</div>
    </div>
</body>
</html>
```
如图:![外边距重叠图](/images/BFC_1.png)

2.解决方案: 在每一个子元素的外层添加一个父元素，并且让父元素生成一个BFC
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        .container {
            border: 1px solid #fff;
            width: 200px;
            height: 200px;
            background: pink;
        }
        .children_1,.children_2 {
            height: 50px;
            line-height: 50px;
            width: 50px;
            text-align: center;
            margin: 20px;
        }
        .children_1 {
            border: 1px solid red;
        }
        .children_2 {
            border: 1px solid blue;
        }
        .bfc_container {
            display: table;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="bfc_container">
            <div class="children_1">box1</div>
        </div>
        <div class="bfc_container">
            <div class="children_2">box2</div>
        </div>
        
    </div>
</body>
</html>
```
如图：![解决外边距重叠](/images/BFC_2.png)


应用二：页面两栏布局，左侧浮动且宽度固定，右侧生成BFC，宽度自适应

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        .left {
            float: left;
            width: 200px;
            height: 100px;
            background-color: red;
        }
        .right {
            height: 100px;
            overflow: hidden;
            background: blue;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="left">左侧浮动、固定宽度</div>
        <div class="right">右侧生成BFC自适应</div>
    </div>
</body>
</html>
```

如图：![两栏布局BFC应用](/images/BFC_3.png)


3.清除浮动: 子元素浮动后，父元素高度为0，可以通过把父元素生成一个BF来解决(BFC计算高度时，浮动元素也参与在内)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        .container {
            overflow: hidden;
        }
        .left {
            float: left;
            width: 200px;
            height: 100px;
            line-height: 100px;
            text-align: center;
            background-color: red;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="left">子元素浮动</div>
    </div>
</body>
</html>
```
如图：
![清除浮动前](/images/BFC_4.png) 
![清除浮动后](/images/BFC_5.png)








