---
title: async+await优雅处理异步 
tags: koa2
categories: koa2
date: 2019-1-7
---

# 前情

```js
app.use(async (ctx, next)=>{
    ctx.body = '1'
    // 下一个中间件
    setTimeout(()=>{
        next()
    },2000)
    ctx.body += '2'
})

app.use(async (ctx, next)=>{
    ctx.body += '3'
    next()
    ctx.body += '4'
})
```

<!--more-->

![](/mdImg/koa5.png)

> 并非异步阻塞，而是当执行到next的时候网络请求已经结束了

例二：

![](/mdImg/koa6.png)

callback回调地狱

![](/mdImg/koa7.png)

# Promise解决

```js
const Koa = require('koa')
const app = new Koa()

function delay(word) {
    // 理解为返回一个承若，两秒钟之后解决,解决之后返回一个word给你
    return new Promise((reslove,reject)=>{
        setTimeout(()=>{
            reslove(word)
        },2000 )
    })
}

delay('科比布莱恩特')
	// .then注册一个结束之后的函数 
    .then((word)=>{
        console.log(word)
    })

app.listen('3000')
```

![](/mdImg/koa8.png)

进阶

```js
const Koa = require('koa')
const app = new Koa()

function delay(word) {
    // 理解为返回一个承若，两秒钟之后解决,解决之后返回一个word给你
    return new Promise((reslove,reject)=>{
        setTimeout(()=>{
            reslove(word)
        },2000 )
    })
}

delay('科比布莱恩特')
    .then((word)=>{
        console.log(word)
        return delay('史蒂芬库里')
    })
    .then((word)=>{
        console.log(word)
        return delay('詹姆斯')
    })
    .then((word)=>{
        console.log(word)
    })
	//.catch错误处理 
app.listen('3000')
```

![](/mdImg/koa9.png)

# async+await优雅解决处理异步

```js
const Koa = require('koa')
const app = new Koa()

function delay(word) {
    // 理解为返回一个承若，两秒钟之后解决,解决之后返回一个word给你
    return new Promise((reslove,reject)=>{
        setTimeout(()=>{
            reslove(word)
        },2000 )
    })
}
// async+await一起使用
async function start(){
    // await在async内部用，等待一个异步执行结束
    const word1 = await delay('科比布莱恩特')
    console.log(word1)
    const word2 = await delay('史蒂夫库里')
    console.log(word2)
    const word3 = await delay('克莱汤普森')
    console.log(word3)
}
start()
app.listen('3000')
```

![](/mdImg/koa10.png)