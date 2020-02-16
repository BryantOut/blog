---
title: koa2入门
tags: koa2
categories: koa2
date: 2019-1-7
---

# 课程内容

- Koa2是什么
- Koa基本用法
- Koa进阶知识

# Koa是什么

- Express原班人马打造，更精简
- Async + await处理异步
- 洋葱圈型的中间件机制

<!--more-->

# Koa最简单的例子

```js
const Koa = require('koa')
const app = new Koa()

app.use(async (ctx, next)=>{
    ctx.body = 'hello koa!'
})

app.listen('3000')
```

# 新建项目

1.

```js
npm init
```

2.

```js
npm install koa --save
```

3.运行

```js
node server.js
```

4.浏览器访问

![](/mdImg/koa2.png)

# 代码疑问

>   麻雀虽小

- ctx是什么【封装了request和response的对象】

- Next是什么【下一个中间件】

  ![](/mdImg/koa3 .png)

- App是什么【启动引用】

# 中间件例子

```js
const Koa = require('koa')
const app = new Koa()

app.use(async (ctx, next)=>{
    ctx.body = '1'
    // 下一个中间件
    next()
    ctx.body += '2'
})

app.use(async (ctx, next)=>{
    ctx.body += '3'
    next()
    ctx.body += '4'
})

app.use(async (ctx, next)=>{
    ctx.body += '5'
    next()
    ctx.body += '6'
})

app.listen('3000')
```

![](/mdImg/koa4 .png)