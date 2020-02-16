---
title: node中的art-template模块
tags: node
categories: node
date: 2018-8-19
---

## 下载

```node
npm install art-template --save
```

![](/mdImg/art-template2.png)

<!--more-->

## 核心方法

### 将模板源代码编译成函数并立刻执行

```js
/* 
将模板源代码编译成函数并立刻执行 
template.render(source, data, options); 
*/
var template = require('art-template')
var fs = require('fs')
fs.readFile(__dirname+"/page/index.html",'utf-8',function (err,data) {
    if (err) console,log('err')
    else {
        var html = template(data,{name:"史蒂芬库里",number:30})
        console.log(html)
    } 
})
```

### 将模板源代码编译成函数

```js
/* 将模板源代码编译成函数 
template.compile(source, options); */
var template = require('art-template')
var fs = require('fs')

fs.readFile(__dirname + "/page/index.html",'utf-8',function (err, data) {
    if (err) console.log('err')
    else {
        var html = template.compile(data)
        console.log(html)
    }
})
```

### 基于模板名渲染模板

```js
/* 基于模板名渲染模板 
template(filename, data); */
var template = require("art-template")
var html = template(__dirname+"/page/index.html",{name:'科比布莱恩特',number:24})
console.log(html)
```

## 定义模板

```html
<tbody id="tbody">
                {{each list}}
                <tr>
                    <td data-value="node_modules/">
                        <a class="icon {{$value.type?'file':'dir'}}" href="javascript:;">{{$value.name}}</a>
                    </td>
                    <td class="detailsColumn" data-value="0">{{$value.type?$value.size:''}}</td>
                    <td class="detailsColumn" data-value="1534676044">{{$value.mtime}}</td>
                </tr>
                {{/each}}
            </tbody>
```

**注意：区别于之前需要包裹** `<script src="text/template"></script>`

## 渲染模板

```js
var html = template(__dirname + "/index.html", {
list: dirInfoList})
res.end(html)
```

## 结合art-template模板引擎在服务器端渲染页面

[art-template官方文档](https://aui.github.io/art-template/docs/)

```js
// 监听用户请求
server.on('request', (req, res) => {
    //__dirname当前模块的文件夹名称
    //readdir读取一个目录的内容。
    var count = 0
    fs.readdir(__dirname, (err, data) => {
        if (err) {
            res.end('err')
        } else {
            // 创建一个数组存储所有文件和文件夹对象
            var dirInfoList = [];
            //遍历数组，获取每一个数组元素所代表的文件或文件夹的详细信息
            data.forEach((value, index, array) => {
                //fs.stat()：可以根据全路径获取文件或文件夹的详细信息
                fs.stat(__dirname + '/' + value, (err2, stat) => {
                    if (err2) res.end('err2')
                    else {
                        count++
                        //每一个stat应该生成页面中的一行数据，一行数据就是一个对象
                        var obj = {}
                        obj.name = value
                        obj.size = stat.size
                        obj.mtime = stat.mtime
                        // isFile函数判断是文件还是文件夹，如果是文件则返回true
                        obj.type = stat.isFile()
                        //将对象存储到数组中
                        dirInfoList.push(obj)

                        // 因为读取文件是异步请求，
                        //所以并不知道什么时候会完成，所以应该在循环内部判断并渲染
                        //判断是否读取完毕
                        if (count == data.length) {
                            var html = template(__dirname + "/index.html", {
                                list: dirInfoList
                            })
                            res.end(html)
                        }
                    }
                })
            })
        }
    })
})
```

