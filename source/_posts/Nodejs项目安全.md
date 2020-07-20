---
title: Nodejs项目安全
tags: node
categories: node
date: 2020-7-16
---

# Nodejs项目安全

- `sql注入`：窃取数据库内容
- `XSS攻击`：窃取前端的cookie内容
- `密码加密`：保障用户信息安全（重要！）
- `server端`攻击方式非常多，预防手段也非常多
- 本课只讲解常见的、能通告 `web server （node.js）层面`预防的
- 有些攻击需要`硬件和服务`来支持（需要OP支持）,如 `DDOS`

<!--more-->

## sql注入

- 最原始、最简单的攻击，从有了web2.0就有了`sql注入攻击`
- 攻击方式：输入一个`sql片段`，最终拼接成一段攻击代码
- 预防措施：使用`mysql的escape函数`处理输入内容即可

#### mysql.escape

```
const { exec, escape } = require('../db/mysql')
const login = (username, password) => {
    username = escape(username)
    password = escape(password)

    const sql = `
        select username, realname from users where username=${username} and password=${password}
    `
    return exec(sql).then(rows => {
        return rows[0] || {}
    })
}

module.exports = {
    login
}
```

![](/mdImg/nodejs11.png)

## XSS攻击

- 前端同学最熟悉的攻击方式，但`server端`更应该掌握
- 攻击方式：在页面展示内容中掺杂js代码，以获取网页信息
- 预防措施：转换生成js的特殊字符

### 转换特殊字符

![](/mdImg/nodejs12.png)

`npm i xss --save`

**blog.js**

```js
const xss = require('xss')
const newBlog = (blogData => {
    // blogData 是一个博客对象，包含 title content author 属性
    const title = xss(blogData.title)
    const content = xss(blogData.content)
    const author = blogData.author
    const createTime = Date.now()

    const sql = `
        insert into blogs (title, content, createtime, author)
        values ('${title}','${content}',${createTime},'${author}')
    `

    return exec(sql).then(insertData => {
        // console.log('insertData is ', insertData)
        return {
            id: insertData.insertId
        }
    })
})
```

![](/mdImg/nodejs13.png)

## 密码加密

- 万一数据库被用户攻破，最不应该泄露的就是用户信息
- 攻击方式：获取用户名和密码，再去尝试登录其他系统
- 预防措施：将密码加密，即便拿到密码也不知道明文

**cryp.js** 

```js
const crypto = require('crypto')

// 密钥
const SECRET_KEY = 'Bryantout_2009242010'

// md5 加密
function md5(content) {
    let md5 = crypto.createHash('md5')
    return md5.update(content).digest('hex')
}

// 加密函数
function genPassword(password) {
    const str = `password=${password}&key=${SECRET_KEY}`
    return md5(str)
}

const res = genPassword('123')
console.log(res)
```

**user.js**

```js
const { exec, escape } = require('../db/mysql')
const { genPassword } = require('../utils/cryp')
const login = (username, password) => {
    // 生成加密密码
    password = genPassword(password)

    username = escape(username)
    password = escape(password)

    const sql = `
        select username, realname from users where username=${username} and password=${password}
    `
    return exec(sql).then(rows => {
        return rows[0] || {}
    })
}

module.exports = {
    login
}
```

![](/mdImg/nodejs14.png)