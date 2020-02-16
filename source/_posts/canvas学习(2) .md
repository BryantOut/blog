---
title: canvas(2)
tags: canvas
categories: canvas
date: 2019-3-23
---

# 绘制平行线&线模糊问题

## 绘制平行线

```html

<body>
    <canvas width="600" height="400"></canvas>
    <script>
        var myCanvas = document.getElementsByTagName('canvas')[0]
        var ctx = myCanvas.getContext('2d')

        ctx.moveTo(100,100)
        ctx.lineTo(300,100)

        ctx.moveTo(100,300)
        ctx.lineTo(300,300)

        ctx.stroke()
    </script>
</body>
```

**效果图**

![](/mdImg/canvas7.png) 

## 思考（关于线条得问题）

1. 默认的宽度是多少 1px

2. 默认的颜色是多少 黑色

3. 产生原因 （但我们看到的是2px 灰色）

   ![](/mdImg/canvas8.png) 


4. 解决方案

   ![](/mdImg/canvas9.png) 

   ![](/mdImg/canvas10.png) 

# 使用圆弧绘制函数 ctx.arc()

```html
<body>
    <canvas width="600" height="400"></canvas>
    <script>
        let myCanvas = document.getElementsByTagName('canvas')[0]
        let ctx = myCanvas.getContext('2d')
        
        /* 绘制圆弧 */
        /* 确定圆心 坐标 x y */
        /* 确定圆的半径  r */
        /* 确定起始位置的位置和结束位置的位置，确定弧和位置 startAngle endAngle */
        /* 取得绘制的方向  direction */

        /* 在中心位置画一个半径150px的圆弧右下角 */
        let w = ctx.canvas.width
        let h = ctx.canvas.height
        ctx.arc(w/2,h/2,150,0,Math.PI/2)
        ctx.stroke()
    </script>
</body>
```

**效果图**

![](/mdImg/canvas11.png) 

# 绘制三条不同颜色和宽度的平行线

```html
<body>
    <canvas width="600" height="400"></canvas>
    <script>
        var myCanvas = document.getElementsByTagName('canvas')[0]
        var ctx = myCanvas.getContext('2d')
        
        /* 蓝色 10px */
        ctx.beginPath() // 开启新路径 -- 解决样式覆盖的问题
        ctx.moveTo(100,100)
        ctx.lineTo(300,100)
        ctx.strokeStyle = '#0094FF'
        ctx.lineWidth = 10
        ctx.stroke()
        
        /* 粉色 20px */
        ctx.beginPath() // 开启新路径 -- 解决样式覆盖的问题
        ctx.moveTo(100,140)
        ctx.lineTo(300,140)
        ctx.strokeStyle = 'pink'
        ctx.lineWidth = 20
        ctx.stroke()
        
        /* 黄色 24px */
        ctx.beginPath() // 开启新路径 -- 解决样式覆盖的问题
        ctx.moveTo(100,200)
        ctx.lineTo(300,200)
        ctx.strokeStyle = 'yellow'
        ctx.lineWidth = 24
        ctx.stroke()
        
        /* 绿色 8px */
        ctx.beginPath() // 开启新路径 -- 解决样式覆盖的问题
        ctx.moveTo(100,300)
        ctx.lineTo(300,300)
        ctx.strokeStyle = 'green'
        ctx.lineWidth = 8
        ctx.stroke()
    </script>
</body>
```

![](/mdImg/canvas12.png)

# 绘制扇形

 ```html
<body>
    <canvas width="600" height="400"></canvas>
    <script>
        let myCanvas = document.getElementsByTagName('canvas')[0]
        let ctx = myCanvas.getContext('2d')
        /* 在中心位置画一个半径150px的圆弧右下角 扇形 边 填充 */
        let w = ctx.canvas.width
        let h = ctx.canvas.height
        /* 把起点放到圆心位置 */
        ctx.moveTo(w/2,h/2)
        ctx.arc(w/2,h/2,150,0,-Math.PI/2,true)
        
        /* 闭合路径 */
        ctx.closePath()
        // 填充
        // ctx.fill()
        /* 闭合路径 */
        ctx.stroke()
    </script>
</body>
 ```

> ctx.closePath() --  填充路径

![](/mdImg/canvas13.png)

> ctx.fill() --  闭合路径

![](/mdImg/canvas14.png)