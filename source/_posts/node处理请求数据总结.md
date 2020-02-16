---
title: node处理请求数据总结
tags: node
categories: node
date: 2018-8-22
---

# Express 常用中间件 body-parser 实现解析

```
$ npm install body-parser
```

> `body-parse` 是非常常用的一个 `express` 中间件，作用是对 post 请求的请求体进行解析。使用非常简单，以下两行代码已经覆盖了大部分的使用场景。

<!--more-->

```js
// 进行参数传递时的配置
var bodyParser = require('body-parser')
// 进行参数传递时的配置
var bodyurl = bodyParser.urlencoded({ extended: false })
app.use(bodyurl)
app.use(bodyParser.json())
```

【注意】这段初始化配置要放在注入路由之后

![](/mdImg/body-parse.png)

```js
【前端代码】
// 为新增按钮注册点击事件
    $('#sub').on('click', (e) => {
        e.preventDefault()
        /* 阻止默认行为，表单中如果type=submit的话，
        点击默认行为是提交表单 */

        //发送ajax请求实现添加用户功能
        $.ajax({
            type:'post',
            url:'/add',
            data:$('form').serialize(),
            dataType:'json',
            success:(result)=>{
                console.log(result)
                alert('添加成功')
                location.href = '/'
            }
        })
    })
```

```js
【后端代码】
// 4.实现新增功能
exports.addHero = (req, res) => {
    getDataModule.addHero(req.body, (err) => {
        } else {
    })
}

console.log(req.body)=>
{ name: '库里',
  gender: '男',
  img: 'upload_478cee4a312b6b129e38a7ec0edf1757.jpeg' }
```

**【注意】req.body会和formidable模块冲突**

[简书相关笔记](https://www.jianshu.com/p/ea0122ad1ac0)

# Node.js之querystring模块 

> querystring从字面上的意思就是查询字符串，一般是对http请求所带的数据进行解析。

1. 首先，使用querystring模块之前，需要require进来：

```js
const querystring = require("querystring");
```

2. 其次，就可以使用模块下的方法：

```js
querystring.parse(str,separator,eq,options)
```

> parse这个方法是将一个字符串反序列化为一个对象。

| 参数           | 描述                                       |
| ------------ | ---------------------------------------- |
| str          | 指需要反序列化的字符串                              |
| separator（可省 | 指用于分割str这个字符串的字符或字符串，默认值为"&";            |
| eq（可省）       | 指用于划分键和值的字符或字符串，默认值为"=";                 |
| options（可省）  | 该参数是一个对象，里面可设置maxKeys和decodeURIComponent这两个属性 |

[详细请看原文笔记](https://www.cnblogs.com/whiteMu/p/5986297.html)

```js
【实践代码--前端部分】
// 为新增按钮注册点击事件
$('#sub').on('click', (e) => {
  e.preventDefault()
  /* 阻止默认行为，表单中如果type=submit的话，
        点击默认行为是提交表单 */

  //发送ajax请求实现添加用户功能
  $.ajax({
    type:'post',
    url:'/add',
    data:$('form').serialize(),
    dataType:'json',
    success:(result)=>{
      console.log(result)
    }
  })
})
```

```js
【后端部分代码】
// 4.实现新增功能
exports.addHero = (req, res) => {
    // console.log(req)
    // 1.接收参数：参数都是字符串
    // 2.在node中允许传递大容量的参数，如果传递的参数较大，那么它支持分批接收参数
    // 3.在接收参数的时候，会持续的触发data事件
    // 4.data事件中有一个回调函数，这个函数的参数就是每次接收到的字符串
    var str = ''
    req.on('data', (chunk) => {
        str += chunk
    })
    // 如果参数接收完毕，会自动触发end事件
    req.on('end', () => {
        //console.log(str)
        //name=sss&gender=%E7%94%B7&img=upload_8766451b2b7779ff7df6f48be075be89.jpg
        //而我们需要的是一个对象
        /*querystring.parse(str,separator,eq,options)
        parse这个方法是将一个字符串反序列化为一个对象。*/
        var newObj = queryString.parse(str)
        // console.log(newObj)
        /* {
            name: '222',
            gender: '女',
            img: 'upload_03a0863ea19392bcaa7c1b03e0427908.jpg'
        } */
        getDataModule.addHero(newObj, (err) => {
            if (err) {
                var ret = {
                    code: 100,
                    msg: '添加英雄失败'
                }
                res.end(JSON.stringify(ret))
            } else {
                var ret = {
                    code: 200,
                    msg: '添加英雄成功'
                }
                res.end(JSON.stringify(ret))
            }
        })
    })
}
```

# Node.js之formidable模块

[formidable模块介绍](http://localhost:4000/2018/08/21/%E5%BC%82%E6%AD%A5%E5%AE%9E%E7%8E%B0%E6%B7%BB%E5%8A%A0%E5%A4%B4%E5%83%8F%E9%A2%84%E8%A7%88/)

# 小知识点强化

1. [JQuery serialize()方法](http://www.runoob.com/jquery/ajax-serialize.html)
2. 在node中允许传递大容量的参数，如果传递的参数较大，那么它支持分批接收参数

```js
exports.addHero = (req, res) => {
    // console.log(req)
    // 1.接收参数：参数都是字符串
    // 2.在node中允许传递大容量的参数，如果传递的参数较大，那么它支持分批接收参数
    // 3.在接收参数的时候，会持续的触发data事件
    // 4.data事件中有一个回调函数，这个函数的参数就是每次接收到的字符串
    var str = ''
    req.on('data', (chunk) => {
        str += chunk
    })
    // 如果参数接收完毕，会自动触发end事件
    req.on('end', () => {}
```

# url模块

> Node.Js中用户URL 格式化和反格式化模块
>
> 用于URL解析、处理等操作的解决方案

`url.parse(urlString,boolean)`

> parse这个方法可以将一个url的字符串解析并返回一个url的对象

| 参数        | 描述                                       |
| --------- | ---------------------------------------- |
| urlString | 指传入一个 url 地址的字符串                         |
| 第二个参数     | （option）传入一个布尔值，默认为 false ,为 true 时，返回的 url 对象中，属性为一个对象 |

**instal**l

```
npm i url
```

**场景**

![](/mdImg/url模块.png)

**基本使用**

```js
var myUrl = require('url')
var delId = myUrl.parse(req.url, true).query.id
```

![](/mdImg/url模块2.png)