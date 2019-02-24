---
title: letCode算法题-字符串
date: 2019-02-23 22:51:32
categories: 
- js
tags:
---


### letCode算法题-字符串

#### 题目一：

描述：

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

示例 1:

输入: "Let's take LeetCode contest"

输出: "s'teL ekat edoCteeL tsetnoc" 

注意：在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。

#### 解法一：


```js
var reverseWords = function(s) {
    
    let arr = s.split(" ");
    
    let result = arr.map(item => {
        return item.split('').reverse().join('');
    });
    
    return result.join(' ');
};
```


对于上面的代码我们还可以对其进行优化，使代码看起来更加优雅。

```js
var reverseWords = function(s) {
    return s.split(' ').map( item => {
        return item.split("").reverse().join("");
    }).join(" ")
};
```



提示：

这里使用了字符串split方法，split方法可以把一个字符串分割为一个数组,如果把""作为separator分割符，那么会把字符串每一个字符都进行分割。当然separator也开始以是一个正则表达式。

[更多详情可以参照菜鸟教程split方法](http://www.runoob.com/jsref/jsref-split.html)

```js
string.split(separator,limit)
```


#### 解法二：


提示：

此种解法使用字符串split方法用正则来匹配空格单词分割

```js
var reverseWords = function(s) {
    return s.split(/\s/g).map(item => {
        return item.split('').reverse().join('');
    }).join(" ");
};
```



#### 解法三：

```js
var reverseWords = function(s) {
    return s.match(/[\w']+/g).map( item => {
        return item.split("").reverse().join("");
    }).join(" ")
};
```


提示：

这里使用了字符串的match方法，可以用来匹配每个单词进行分割。

```js
string.match(regexp)
```

参数：

|参数|描述|
|:---:|:---:|
|regexp|必需，规定要匹配的RegExp对象，如果该参数不是RegExp对象，可以使用RegExp将其转化为RegExp对象|


返回值：

返回值为一个数组，用来存放匹配到的结果，,如果没有找到则返回null。

[1. 更多详情可以参考菜鸟教程match](http://www.runoob.com/jsref/jsref-match.html)

[2. 菜鸟教程JavaScript RegExp 对象](http://www.runoob.com/js/js-obj-regexp.html)


[3. 菜鸟教程RegExp参考手册](http://www.runoob.com/jsref/jsref-obj-regexp.html)




#### 题目二：

给定一个字符串 s，计算具有相同数量0和1的非空(连续)子字符串的数量，并且这些子字符串中的所有0和所有1都是组合在一起的。

重复出现的子串要计算它们出现的次数。

示例 1 :

输入: "00110011"

输出: 6

解释: 有6个子串具有相同数量的连续1和0：“0011”，“01”，“1100”，“10”，“0011” 和 “01”。

请注意，一些重复出现的子串要计算它们出现的次数。

另外，“00110011”不是有效的子串，因为所有的0（和1）没有组合在一起。

解法：


```js

function f(str) {
    let result = [];

    let match = (s) => {
        // 匹配连续多个0或者多个1，并且取第一个
        let j = s.match(/(0+|1+)/)[0];

        // 如果取到的是0，就进行取反，然后获取长度和j一样
        let o = (j[0] ^ 1).toString().repeat(j.length);

        let reg = new RegExp(`^(${j}${o})`);

        // 如果在子字符串中可以找到匹配的，则返回，否则返回空字符串
        if(reg.test(s)) {
            return RegExp.$1;
        }else {
            return '';
        }
    }

    for(let i = 0; i < str.length - 1; i++) {
        // 从 i 开始截取到最后一个
        let sub = match(str.slice(i));
        if(sub) {
            result.push(sub);
        }
    }

    return result;
}
```

#### 提示1：

这里使用到了test()方法：

test()方法搜索字符串指定的值，根据结果并返回真或假。

下面的示例是从字符串中搜索字符 "e" ：

实例

var patt1=new RegExp("e");

document.write(patt1.test("The best things in life are free"));

由于该字符串中存在字母 "e"，以上代码的输出将是：true


#### 提示2：

这里使用到了字符串的repeat方法，repeat() 方法字符串复制指定次数。

```js
string.repeat(count)
```

