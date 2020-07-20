---
title: nodejs--日志
tags: node
categories: node
date: 2020-7-15
---

# 日志

- 系统没有日志，就等于人没有眼睛--抓瞎
- 第一，访问日志 `assess log` (`server 端`最重要的日志)
- 第二，自定义日志 (包括自定义事件、错误记录等)
- `nodejs` 文件操作， `nodejs stream`
- 日志功能开发和使用
- 日志文件拆分，日志内容分析
- 日志要存储到文件中
- 为何不存储到`mysql`中？
- 为何不存储到`redis`中？

<!--more-->

## 操作文件基础命令

```js
const fs = require('fs')
const path = require('path')

const fileName = path.resolve(__dirname, 'data.txt')

// 读取文件内容
// fs.readFile(fileName, (err, data) => {
//     if (err) {
//         console.log(err)
//         return
//     }
//     // data 是二进制类型，需要转换为字符串
//     console.log(data.toString())
// })

// 写入文件
// const content = '\n这是新写入的内容'
// const opt = {
//     flag: 'a' // 追加写入，覆盖用 'w'
// }
// fs.writeFile(fileName, content, opt, (err) => {
//     if (err) {
//         console.log(err)
//     }
// })

// 判断文件是否存在
fs.exists(fileName, (exists) => {
    console.log('exists', exists)
})
```

## stream 介绍

#### `IO` 操作的性能瓶颈

- `IO`包括"`网络IO`"和"`文件IO`"
- 相比于CPU计算和内存读写，`IO`的突出特点就是：**慢！**
- 如何在有限的硬件资源下提高`IO`的操作效率？

![](/mdImg/nodejs08.png)

#### stream的简单例子

```js
// 例1--标准输入输出
process.stdin.pipe(process.stdout)
```

```js
// 例2
const http = require('http')
const server = http.createServer((req, res) => {
    if (req.method === 'POST') {
        req.pipe(res) // 最主要
    }
})

server.listen(8000)
```

```js
// 例3
const fs = require('fs')
const path = require('path')

const fileName1 = path.resolve(__dirname, 'data.txt')
const fileName2 = path.resolve(__dirname, 'data-bak.txt')

const readStream = fs.createReadStream(fileName1)
const writeStream = fs.createWriteStream(fileName2)

readStream.pipe(writeStream)

readStream.on('data', chunk => {
    console.log(chunk.toString())
})

readStream.on('end', () => {
    console.log('copy done')
})
```

```js
// 例四--http 请求
const http = require('http')
const fs = require('fs')
const path = require('path')
const fileName1 = path.resolve(__dirname, 'data.txt')
const server = http.createServer((req, res) => {
    if (req.method === 'GET') {
        const readStream = fs.createReadStream(fileName1)
        readStream.pipe(res)
    }
})
server.listen(8000)
```

## 写日志

```js
// log.js
const fs = require('fs')
const path = require('path')

// 写日志
function writeLog(writeStream, log) {
    writeStream.write(log + '\n') // 关键代码
}

// 生成 write Stream
function createWriteStream(fileName) {
    const fullFileName = path.join(__dirname, '../', '../', 'logs', fileName)
    const writeStream = fs.createWriteStream(fullFileName, {
        flags: 'a'
    })
    return writeStream
}

// 写访问日志
const accessWriteStream = createWriteStream('access.log')

function access(log) {
    // if (process.env.NODE_ENV !== 'dev') {
    //     writeLog(accessWriteStream, log)
    // } else {
    //     console.log(log)
    // }
    writeLog(accessWriteStream, log)
}

module.exports = {
    access
}
```

```js
// app.js
const serverHandle = (req, res) => {
    // 记录 access log
    access(`${req.method} -- ${req.url} -- ${req.headers['user-agent']} -- ${Date.now()}`)
    // ……
}
```

## 日志拆分

- 日志内容会慢慢积累，放在一个文件中不好处理
- 按时间拆分日志，如`2019-02-10.access.log`
- 实现方式: `linux` 的 `crontab`   命令，即定时任务

**copy.sh**

```js
#!/bin/sh
cd /gitFile/nodejsLearning/blog1.0/logs
cp access.log $(date +%Y-%m-%d).access.log
echo "" > access.log
```

### crontab

- 设置定是个任务，格式 `*****command`
- 将`access.log`拷贝并重命名为`2019-02-10.access.log`
- 清空`access.log`文件，继续积累日志

### 日志分析

- 如针对 `access.log`日志，分析`chrome的占比`
- 日志是按行存储的，一行就是一条日志
- 试用`node.js`的`readline`（基于`stream`，效率高）

**readline.js**

```js
const fs = require('fs')
const path = require('path')
const readline = require('readline')

// 文件名
const fileName = path.join(__dirname, '../', '../', 'logs', 'access.log')

// 创建 read stream
const readStream = fs.createReadStream(fileName)

// 创建 readline 对象
const rl = readline.createInterface({
    input: readStream
})

let sum = 0
let chromeNum = 0

// 逐行读取
rl.on('line', (lineData) => {
    if (!lineData) {
        return
    }

    // 记录总行数
    sum++

    const arr = lineData.split('--')
    if (arr[2] && arr[2].indexOf('Chrome') > 0) {
        // 累加 chrome 的数量
        chromeNum++
    }
})

// 监听读取完成
rl.on('close', () => {
    console.log('chrome 占比：' + chromeNum / sum)
})
```

