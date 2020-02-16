---
title: Node.js连接mysql
tags: node
categories: node
date: 2018-8-23
---

> 本篇我们将为大家介绍如何使用 Node.js 来连接 MySQL,并对数据库进行操作。

```js
npm i mysql /*安装驱动*/
```

<!--more-->

**连接数据库**

```js
var mysql = require('mysql')
// 创建模块
var connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'herosdb'
})
connection.connect()
```

**实例代码对数据库进行操作**

```js
【查】
// 1. 获取所有数据
exports.getAllData = (callback) => {
    // .创建查询语句
    var sql = 'select id,name,gender,img from heros where isDelete = 0'
    // 执行查询命令
    connection.query(sql, (err, result) => {
        if (err) {
            callback(err)
        } else {
            callback(null, { heros: result })
        }
    })
}
```

```js
【增】
// 添加新英雄数据到json文件中
exports.addHero = (newObj, callback) => {
    // 创建查询语句
    var sql = `insert into heros values(null,'${newObj.name}','${newObj.gender}','${newObj.img}',default)`
    // 执行查询命令
    connection.query(sql, (err, result) => {
        if (err) callback(err)
        else {
            callback(null, result)
        }
    })
}
```

```js
【更】
// 更新用户信息
exports.uploadHeroInfo = (editObj, callback) => {
    var sql = `update heros set ? where id = ?`
    connection.query(sql, [editObj, editObj.id], (err, result) => {
        if (err) callback(err)
        else {
            callback(null)
        }
    })
}
```

```js
【删】
// 删除用户
exports.delHero = (id, callback) => {
    var sql = `update heros set isDel = 1 where id = ?`
    connection.query(sql, [id], (err, result) => {
        if (err) callback(err)
        else {
            // 这里获取的是数组但是调用者需要的是一个对象，同时我们发现这个sql语句如果执行成功，只可能会有一条记录
            callback(null)
        }
    })
}
```



