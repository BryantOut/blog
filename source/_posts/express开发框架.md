---
title: express开发框架
tags: node
categories: node
date: 2018-8-23
---

[Express官方首页](http://www.expressjs.com.cn/)

![](/mdImg/express框架学习1.png)

```js
$ npm install express --save
```

# Hello world实例

```js
var express = require('express');
var app = express();// app就是一个服务器
// 设置服务器监听端口
app.listen(0824, () => {
    console.log('http://127.0.0.1:3003')
})
app.get('/', function (req, res) {
    res.end('hello world');
});
```

------

<!--more-->

# 一个简单的 Express 路由

```js
// 对网站首页的访问返回 "Hello World!" 字样
app.get('/', function (req, res) {
  res.end('Hello World!');
});

// 网站首页接受 POST 请求
app.post('/', function (req, res) {
  res.end('Got a POST request');
});

// /user 节点接受 PUT 请求
app.put('/user', function (req, res) {
  res.end('Got a PUT request at /user');
});

// /user 节点接受 DELETE 请求
app.delete('/user', function (req, res) {
  res.end('Got a DELETE request at /user');
});
```

------

## 在 router.js 制定路由规则

```js
var express = require('express')
// 创建路由模块对象
var router = express.Router()

//引入数据处理模块
var handler = require('./handler')

router.get('/', handler.getIndexPage)
    .get('/add', handler.getAddPage)
    .get('/fileUpload', handler.doFileUpload)
    .post('/add', handler.addHero)
    .get('/edit', handler.getEditPage)
    .post('/edit', handler.doEdit)
    .get('/del', handler.doDel)


// 暴露成员
module.exports = router
```

## 注入路由

```js
var express = require('express')
var router = require('./router');
var app = express()// app就是一个服务器

// 静态文件托管
app.use(express.static('public'))

// 设置服务器监听端口
app.listen(3008, () => {
    console.log('http://127.0.0.1:3008')
})

// 让当前应用使用我们定制的路由规则
// 挂载--use
// 注入路由
app.use(router)
```

# 利用 Express 托管静态文件

通过 Express 内置的 `express.static` 可以方便地托管静态文件，例如图片、CSS、JavaScript 文件等。

将静态资源文件所在的目录作为参数传递给 `express.static` 中间件就可以提供静态资源文件的访问了。例如，假设在 `public` 目录放置了图片、CSS 和 JavaScript 文件，你就可以：

```js
app.use(express.static('public'));
```

## 静态文件托管

```js
// 静态文件托管
app.use(express.static('public'))
```

![](/mdImg/express框架学习2.png)

```js
/*所有文件的路径都是相对于存放目录的，因此，存放静态文件的目录名不会出现在 URL 中。*/
```

>  如果你的静态资源存放在多个目录下面，你可以多次调用 `express.static` 中间件：

```js
app.use(express.static('public'));
app.use(express.static('files'));
```

> 访问静态资源文件时，`express.static` 中间件会根据目录添加的顺序查找所需的文件。

如果你希望所有通过 `express.static` 访问的文件都存放在一个“虚拟（virtual）”目录（即目录根本不存在）下面，可以通过为静态资源目录[指定一个挂载路径](http://www.expressjs.com.cn/4x/api.html#app.use)的方式来实现，如下所示：

```js
app.use('/static', express.static('public'));
```

现在，你就爱可以通过带有 “/static” 前缀的地址来访问 `public` 目录下面的文件了。

```js
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html
```

