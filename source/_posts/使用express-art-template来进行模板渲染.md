---
title: 使用express-art-template来进行模板渲染
tags: node
categories: node
date: 2018-8-23
---

# express-art-template

> art-template for express

【install】

```js
npm install --save art-template
npm install --save express-art-template
```

<!--more-->

【init】

```js
// 下面这个中间件就是用来告诉当前应用要使用express-art-template来进行模板的渲染
// art:是指能够用于解析的文件的扩展名，我们可以修改其为html
app.engine('html', require('express-art-template'))
app.set('view options', {
    debug: process.env.NODE_ENV !== 'production'
})
```

## render

>  它的作用就是能够实现模板引擎的渲染，并且将渲染结果返回

```js
render(模板路径，数据)
```

> 我们现在使用到了模板引擎，它需要再引入一个新的引擎模块，名字叫做 `express-art-template`

【注意】我们现在是使用了 `express-art-template` ，但是必须强调的是，这不是一个唯一的选择。只是你应该能够理解到，这个 `render` 必须有模板引擎的参与

> 所以它需要引入两个模板引擎模块：art-template | express-art-template

```js
exports.getIndexPage = (req, res) => {
    getDataModule.getDataModule((err,data)=>{
        if (err) {
            res.end('err')
        } else {
            res.render(__dirname + "/page/index.html", data)
        }
    })
}
```

```js
exports.getDataModule = (callback) => {
    // 创建查询语句
    var sql = 'select id,name,gender,img from heros where isDel = 0';
    // 执行查询命令
    connection.query(sql, (err, result) => {
        if (err) {
            callback(err)
        } else {
            callback(null, {
                heros: result
            })
        }
    })
}
```

![](/mdImg/bg.jpg)

