---
title: 微信小程序分享canvas总结
date: 2019-07-03 11:09:49
tags:
categories:
    - js
---

#### 一、自动计算画布元素高度：

```js
//绘制自动计算目前位置
export class Queue {
    constructor(top = 0) {
        this.queue = [];
        this.startTop = top;
        this.endBottom = this.startTop;
    }
    push(fn, style) {
        let y = this.startTop;
        let prev = this.queue[this.queue.length - 1] || null;
        if (prev) {
            let { height = 0, marginBottom = 0 } = prev.style;
            y = prev.y + height + marginBottom;
        }
        if (style.marginTop) {
            y += style.marginTop;
        }
        let { height = 0, marginBottom = 0 } = style;
        this.endBottom = y + height + marginBottom;
        this.queue.push({
            y,
            style,
            fn
        });
    }
    async run() {
        if (this.queue.length > 0) {
            await Promise.all(
                this.queue.map(item => {
                    return item.fn(item.y);
                })
            );
        }
        return await Promise.resolve();
    }
}
```

具体使用：

```js
<canvas
    style="width: 375px; height: 1000px;"
    canvas-id="canvas"
    class="canvas"
/>;
const context = wx.createCanvasContext("canvas", this);
// 首先new一个对象queue_2,top为距离画布圆点上方的距离
let top = 10;
let queue_2 = new Queue(top);

await drawDashLineAndCircle(context, queue_2);
async function drawDashLineAndCircle(context, queue_2) {
    queue_2.push(
        async function(top) {
            context.arc(24, top + 40, 10, 0, 2 * Math.PI);
            context.setFillStyle("#FFC800");
            context.fill();
            context.arc(24 + 327, top + 40, 10, 0, 2 * Math.PI);
            context.setFillStyle("#FFC800");
            context.fill();
        },
        {
            height: 30,
            marginBottom: 20
        }
    );
}

// 最后执行
await queue_2.run();
```
