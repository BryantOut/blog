---
title: express应用级中间件，Cookie及Sesssion的使用
tags: node
categories: node
date: 2018-8-25
---

# Express 应用级中间件

```js
// 没有挂载路径的中间件，应用的每个请求都会执行该中间件
app.use(function (req, res, next) {
    if (req.session.isLogin && req.session.isLogin === 'true' || req.url === '/login' || req.url === '/dologin') {
        // 继续当前的路由请求
        console.log(123)
        next()
    } else {
        console.log(999)
        res.writeHead(301, {
            'Location': '/login'
        })
        res.end()
    }
})
```

<!--more-->

# Cookie 的使用

>  如何获取cookie中的数据

```js
// 获取 cookie:发送请求的时候获取客户端传递的 cookie
// 获取 cookie 通过 req 获取 -- 字符串
var mycookie = req.headers.cookie
```

> 如何设置cookie的数据

```js'
// 设置 cookie:通过响应头设置 cookie
// cookie：一般是键值对字符串
res.writeHead(200, {
  // 'Set-Cookie':'username=jack,age=20'
  // 写入多组 cookie 值：通过['','','']
  'Set-Cookie': ['isLogin=true;expires' + expires, 'username=bryant']
})
```

# Session 的使用

> 需要引入的文件

```js
// 使用express来实现创建服务器和响应用户请求
var express = require('express')
// 引入session
var session = require('express-session')
```

> 服务器默认不会使用 session 来进行状态保持，如果在 Express 中向使用 session 那么就需要手动设置

```js
app.use(session({
    secret: 'secret', // 对session id 相关的cookie 进行签名 -- 加盐
    resave: false, //不管session数据是否发生改变，都会自动保存
    saveUninitialized: false, // 是否保存未初始化的会话
}));
```

> 获取 session 存的值

```js
req.session.isLogin
```

> 设置 session 中的值

```js
req.session.isLogin = 'true'
req.session.currentUser = {
  'name': 'Bryant',
  'number': 24
}
```

