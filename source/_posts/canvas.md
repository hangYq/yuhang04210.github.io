---
title: canvas
date: 2019-07-09 11:44:02
tags:
---

### canvas

#### 一、 canvas 绘制五角星

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>
    </head>
    <body>
        <p onClick="draw()">绘制</p>
        <canvas id="canvas" style="width: 500px;height:500px"></canvas>
        <script>
            function draw() {
                let canvas = document.getElementById("canvas");
                let context = canvas.getContext("2d");
                drawStar(context, 25, 10, 100, 100, 0, 1, "#FFC91F", "#FFC91F");
            }

            function drawStar(
                ctx,
                R,
                r,
                x,
                y,
                rot,
                borderWidth,
                borderStyle,
                fillStyle
            ) {
                ctx.beginPath();
                for (var i = 0; i < 5; i++) {
                    ctx.lineTo(
                        Math.cos(((18 + 72 * i - rot) / 180) * Math.PI) * R + x,
                        -Math.sin(((18 + 72 * i - rot) / 180) * Math.PI) * R + y
                    );
                    ctx.lineTo(
                        Math.cos(((54 + 72 * i - rot) / 180) * Math.PI) * r + x,
                        -Math.sin(((54 + 72 * i - rot) / 180) * Math.PI) * r + y
                    );
                }
                ctx.closePath();
                ctx.lineWidth = borderWidth;
                ctx.strokeStyle = borderStyle;
                ctx.fillStyle = fillStyle;

                ctx.fill();
                ctx.stroke();
            }
        </script>
    </body>
</html>
```

#### 二、绘制圆角矩形

```js
/**
 *绘制圆角矩形
 * @param {*} context canvas 上下文
 * @param {*} x 起始点x坐标
 * @param {*} y 起始点y坐标
 * @param {*} width 长方形宽度
 * @param {*} height 长方形高度
 * @param {*} r 圆角半径
 * @param {*} lineWidth 线宽度 （可选，默认为1）
 * @param {*} strokeStyle 线颜色 （可选，默认为#000）
 */
function drawRoundRect(
    context,
    x,
    y,
    width,
    height,
    r,
    lineWidth = 1,
    strokeStyle = "#000"
) {
    context.beginPath();
    context.moveTo(x, y + r);
    context.lineTo(x, y + height - r);
    context.quadraticCurveTo(x, y + height, x + r, y + height);
    context.lineTo(x + width - r, y + height);
    context.quadraticCurveTo(x + width, y + height, x + width, y + height - r);
    context.lineTo(x + width, y + r);
    context.quadraticCurveTo(x + width, y, x + width - r, y);
    context.lineTo(x + r, y);
    context.quadraticCurveTo(x, y, x, y + r);
    context.lineWidth = lineWidth;
    context.strokeStyle = strokeStyle;

    context.stroke();
}
// 示例
drawRoundRect(context, 10, 10, 100, 100, 10);
drawRoundRect(context, 10, 10, 100, 100, 10, 5, "red");
```

#### 三 、canvas 中处理文字换行转化

```js
/* 文字换行转化
 * @params text: string 文字
 *         num : number 单行字节长度
 * @return [处理文字的总字节长度: number，每行显示内容的数组: Array<string>，行数: number]
 */
function textByteLength(text, num) {
    // text为传入的文本  num为单行显示的字节长度
    if (typeof text !== "string" || text === "") {
        return [0, [], 0];
    }
    text = text.replace(/[\n\r]/g, "");
    let strLength = 0; // text byte length
    let rows = 1;
    let str = 0;
    let arr = [];
    for (let j = 0; j < text.length; j++) {
        if (text.charCodeAt(j) > 255) {
            strLength += 2;
            if (strLength > rows * num) {
                arr.push(text.slice(str, j));
                str = j;
                rows++;
            }
        } else {
            strLength++;
            if (strLength > rows * num) {
                arr.push(text.slice(str, j));
                str = j;
                rows++;
            }
        }
    }
    arr.push(text.slice(str, text.length));
    return [strLength, arr, rows]; //  [处理文字的总字节长度，每行显示内容的数组，行数]
}
var text = "hello word hello word hello wrod";

var a = textByteLength(text, 6); // [32, ["hello ", "word h", "ello w", "ord he", "llo wr", "od"], 6 ]
```
