---
title: canvas(1)
tags: canvas
categories: canvas
date: 2019-3-23
---

# 课前思考(解释什么是弧度)

![](/mdImg/canvas1.png)

## 体验Canvas

### 什么是Canvas？

> HTML5 的 canvas 元素使用 JavaScript 在网页上绘制图像。  
> 画布是一个矩形区域，您可以控制其每一像素。  
> canvas 拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。

### 创建Canvas元素

向 HTML5 页面添加 canvas 元素。  
规定元素的 id、宽度和高度：

```html
<canvas id="myCanvas" width="200" height="100"></canvas>
```

## 初体验

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        canvas {
            border : 1px solid #000;
            /* 不建议在样式设置尺寸 */
        }
    </style>
</head>
<body>
    <!-- 1.准备画布 -->
        <!-- 1.1 画布是白色得，而且默认300*150 -->
        <!-- 1.2 设置画布的大小 width="600" height="400" -->
    <canvas width="600" height="400"></canvas>
    <!-- 2.准备绘制工具 -->
    <!-- 3.利用工具绘图 -->
    <script>
        /* 1.获取元素 */
        var myCanvas = document.getElementsByTagName('canvas')[0]
        /* 2.获取上下文 绘制工具箱 */
        var ctx = myCanvas.getContext('2d')
        /* 3.绘制一条直线 */
        ctx.moveTo(100,100) /* 起点坐标，上100，左100 */
        /* 4.绘制直线(轨迹，绘制路径，是看不见的) */
        ctx.lineTo(200,100)
        /* 5.描边 */
        ctx.stroke()
    </script>
</body>
</html>
```

![](/mdImg/canvas2.png)

# 解释上下文和绘制路径

# 理解曲线的绘制

![](/mdImg/canvas3.png)

**效果图**

![](/mdImg/canvas4.png)

>  不同公式

![](/mdImgcanvas5.png)

![](/mdImgcanvas6.png)



