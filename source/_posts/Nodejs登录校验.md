---
title: nodejs--登录校验
tags: node
categories: node
date: 2020-7-13
---

# 登录

1. 核心：登录校验&登录信息存储
2. 为何只讲登录，不讲注册？

## 目录

1. `cookie`和`session`
2. `session`写入`redis`
3. 开发登录功能，和前端联调（用`nginx`反向代理）

<!--more-->

### cookie

> 什么是`cookie`

- 存储在浏览器中的一段字符串（最大5kb）
- 跨域不共享
- 格式如`k1=v1;k2=v2;k3=v3;`因此可以存储结构化数据
- 每次发送`http`请求，会将请求域的`cookie`一起发送给`server`
- `server`可以修改`cookie`并返回给浏览器
- 浏览器中也可以通告`js`修改`cookie`(有限制)

![](/mdImg/nodejs01.png)

> `js` 操作 `cookie`,浏览器中查看`cookie`

`document.cookie`

> `server`端操作`cookie`，实现登录验证

```js
// 解析cookie
req.cookie = {}
const cookieStr = req.headers.cookie || '' // k1=v1;k2=v2;……
cookieStr.split(';').forEach(item=>{
    if(!item) {
        return
    }
    const arr = item.split('=')
    const key = arr[0].trim()
    const val = arr[1].trim()
    req.cookie[key] = val
})
```

```js
// 登录验证的测试
if(method === 'GET' && req.path === '/api/user/login-test') {
    if(req.cookie.username) {
        return Promise.resolve(new SuccessModel({
            username: req.cookie.username
        }))
    }
    return Promise.resolve(new ErrorModel('尚未登录'))
}

module.exports = handleUserRouter
```

```js
// 获取 cookie 的过期时间
const getCookieExpires = () => {
    const d = new Date()
    d.setTime(d.getTime() + (24 * 60 * 60 * 1000))
    return d.toGMTString()
}

// 登录后端生成cookie
if(method === 'GET' && req.path === '/api/user/login') {
    const {username,password} = req.query
    const result = login(username,password)
    return result.then(data=>{
        if(data.username) {
            // 操作 cookie
            res.setHeader('Set-Cookie',`username=${data.username}; path=/; httpOnly; expires=${getCookieExpires()}`)
        }
    })
}
```

### session

- 用`cookie`存储登录信息的问题：会暴露`username`，很危险
- 如何解决: `cookie`中存储`userid`,`server端`对应`username`
- 解决方案:`session`,即`server端`存储用户信息

![](/mdImg/nodejs02.png)

```js
// session 数据
const SESSION_DATA = {}

// 解析 scssion    
let needSetCookie = false
let userId = req.cookie.userid
console.log(SESSION_DATA)
if (userId) {
    if (!SESSION_DATA[userId]) {
        SESSION_DATA[userId] = {}
    }
} else {
    needSetCookie = true
    userId = `${Date.now()}_${Math.random()}`
    SESSION_DATA[userId] = {}
}
req.session = SESSION_DATA
```

```js
// 处理 blog 路由
const blogResult = handleBlogRouter(req, res)
if (blogResult) {
    blogResult.then(blogData => {
        if (needSetCookie) {
            res.setHeader('Set-Cookie', `userid=${userId}; path=/; httpOnly; expires=${getCookieExpires()}`)
        }
        res.end(
            JSON.stringify(blogData)
        )
    })
    return
}
```

```js
// 登录验证的测试
if (method === 'POST' && req.path === '/api/user/login') {
    const { username, password } = req.body
    // let { username, password } = req.query
    const result = login(username, password)
    return result.then(data => {
        if (data.username) {
            // 设置 session
            req.session.username = data.username
            req.session.realname = data.realname
            console.log(req.session)

            return new SuccessModel()
        }
        return new ErrorModel('登录失败')
    })
}
```

```js
// 登录验证的测试
if(method === 'GET' && req.path === '/api/user/login-test') {
    if(req.session.username) {
        return Promise.resolve(new SuccessModel({
            username: req.cookie.username
        }))
    }
    return Promise.resolve(new ErrorModel('尚未登录'))
}

module.exports = handleUserRouter
```

### redis

#### 从 session 到 redis

- 目前`session`直接是`js`变量，放在`nodejs`进程内存中

- 第一，进程内存有限，访问量过大，内存暴增怎么办？

  ![](/mdImg/nodejs04.png)

- 第二，正式上线运行时多进程，进程之间内存无法共享

  ![](/mdImg/nodejs05.png)

> 0x1000(程序内存分配起始地址)--0x8000(程序内存分配结束地址)
>
> stack(栈):一般存储程序运行的一些基础变量
>
> heap(栈):一般存储引用数据类型

 ![](/mdImg/nodejs03.png)

#### 解决方案 redis(内存数据库)

- `web server` 最常用的缓存数据库，数据存放在内存中
- 相比于`mysql`，访问速度快（内存和硬盘不是一个数量级的）
- 但是成本更高，可存储的数据量更少（内存的硬伤）

![](/mdImg/nodejs06.png)

- 将`web server`和`redis`拆分为两个单独的服务
- 双方都是独立的，都是可扩展（例如都扩展成集群）
- （包括`mysql`，也是一个单独的服务，也可扩展）

#### 为何session适合用redis?

- `session`访问频繁，对性能要求极高
- `session`可不考虑断电丢失的问题（内存的硬伤）
- `session`数据量不会太大（相比于`mysql`中存储的数据）

#### 为何网站数据不适合用redis?

- 操作频率不是太高（相比于`session`操作）
- 断电不能丢失，必须保留
- 数据量太大，内存成本太高

#### 安装

[windows](https://www.runoob.com/redis/redis-install.html)

`npm i redis --save`

#### demo.js

```js
const redis = require('redis')

// 创建客户端
const redisClient = redis.createClient(6379, '127.0.0.1')
redisClient.on('error', err => {
    console.error(err)
})

// 测试
redisClient.set('myname', 'bryantout', redis.print)
redisClient.get('myname', (err, val) => {
    if (err) {
        console.error(err)
        return
    }
    console.log('val', val)

    // 退出
    redisClient.quit()
}
```

#### 项目代码

```js
// db.js

const env = process.env.NODE_ENV // 环境参数

// 配置
let REDIS_CONF

if (env === 'dev') {
    // redis
    REDIS_CONF = {
        port: 6379,
        host: '127.0.0.1'
    }
}

if (env === 'production') {
    // redis
    REDIS_CONF = {
        port: 6379,
        host: '127.0.0.1'
    }
}

module.exports = {
    REDIS_CONF
}
```

```js
// redis.js
const redis = require('redis')
const { REDIS_CONF } = require('../config/db')

// 创建客户端
const redisClient = redis.createClient(REDIS_CONF.port, REDIS_CONF.host)
redisClient.on('error', err => {
    console.error(err)
})

function set(key, val) {
    if (typeof val === 'object') {
        val = JSON.stringify(val)
    }
    redisClient.set(key, val, redis.print)
}

function get(key) {
    const promise = new Promise((resolve, reject) => {
        redisClient.get(key, (err, val) => {
            if (err) {
                reject(err)
                return
            }
            // resolve(val)
            if (val === null) {
                resolve(null)
                return
            }

            try {
                resolve(
                    JSON.parse(val)
                )
            } catch (ex) {
                resolve(val)
            }
        })
    })
    return promise
}

module.exports = {
    set,
    get
}
```

```js
const {
    get,
    set
} = require('./src/db/redis')

// 解析 session (使用 resis )
let needSetCookie = false
let userId = req.cookie.userid
if (!userId) {
    needSetCookie = true
    userId = `${Date.now()}_${Math.random()}`
    // 初始化 redis 中的 session 值
    set(userId, {})
}
// 获取 session
req.sessionId = userId
get(req.sessionId).then(sessionData => {
    if (sessionData == null) {
        // 初始化 redis 中的 session 值
        set(req.sessionId, {})
        // 设置 session
        req.session = {}
    } else {
        // 设置 session
        req.session = sessionData
    }
    // console.log('req.session ', req.session)

    // 处理 post data
    return getPostData(req)
}).then(postData => {
    // ……
})
```

### nginx

#### 和前端联调

![](/mdImg/nodejs07.png)

- 登录功能依赖`cookie`，必须用浏览器来联调
- `cookie`跨域不共享，前端和`server端`必须同域
- 需要用到`nginx`做代理，让前后端端同域

#### 安装

[下载地址](http://nginx.org/en/download.html)

[nginx反向代理与负载均衡教程](https://www.bilibili.com/video/BV1Bx411Z7Do?from=search&seid=5120936995092259818)

[OpenResty下载地址](http://openresty.org/cn/download.html)

[Nginx在windows下常用命令](https://www.cnblogs.com/hr-cmbc/p/10899184.html)

#### nginx.conf

```js
worker_processes  1;

events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       8080;
        server_name  localhost;
		default_type text/html;
		
		location / {
			proxy_pass http://localhost:8001;
		}

		location /api/ {
			proxy_pass http://localhost:8000;
			proxy_set_header Host $host;
		}
    }
}
```

