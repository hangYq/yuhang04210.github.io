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
