---
title: Node.js--learning入门
tags: node
categories: node
date: 2018-8-18
---
> Node.js 是一个基于 Chrome V8 引擎的 JavaScript运行环境。

# Node的事件驱动模型

[原创作者](https://blog.csdn.net/u013217071/article/details/78043081)

## **主线程：**

1. 执行node的代码，把代码放入队列
2. 事件循环程序（主线程）把队列里面的同步代码都执行了
3. 同步代码执行完成，执行异步代码
4. 异步代码分2种状况

- 异步非io(setTimeout()、setInterval()) :判断是否可以执行，如果可以执行就执行，不可以跳过。
- 异步io(文件操作)：会从线程池当中去取出一条线程，帮助主线程去执行。

5. 主线程会一直**轮询**，队列中没有代码了，主线程就会退出。

<!--more-->

## **子线程：**

1. 在线程池李休息
2. 异步io的操作来了，执行异步io操作
3. 子线程会把异步io操作的callback函数，扔回给队列
4. 子线程会回到线程池去休息
5. callback:在异步io代码执行完成的时候被扔回主线程

## **补充：**

### **什么是进程？**

1. 进程为运行当中的应用程序提供运行环境的
2. 一个运行当中的应用程序就会有一个进程与之相对应

### **什么是线程？**

1. 线程是用来运行应用程序代码的
2. 一个线程在一个时间只能做一件事情
3. 多线程，调用起来很麻烦
4. node是单线程执行，用异步替换了多线程

### **同步、异步有什么不同？**

1. 异步不会阻塞后面的代码
2. 一条线程会执行同步的代码后执行异步的代码

### **异步和多线程的比较？**

1. node的异步是帮助我们去做了多线程的操作，简化了代码。

# Node.js的包管理器 npm

> npm 是全球最大的开源库生态系统

## 初始化包管理文件

```js
npm init -y
```

## 安装包

```js
npm install 包名 --save/--save-dev;
/*
  install 可以简写成 i
  --save可以简写成-S
  --save-dev可以简写成-D
  --save表示把包安装到部署依赖中（在开发和部署上线都需要使用的包）
  --save-dev表示安装到开发依赖（只在项目开发阶段需要用到的包）
*/
```

**安装全局包**

```js
npm i webpack -g
/*
  其中，
  -g表示全局安装某些包，
  通过-g安装的包都在(C:\Users\用户名\AppData\Roaming\npm)
*/
```

# 如何设置 npm 的安装镜像

## 什么是 npm 淘宝镜像呢

​	由于每次安装包需要走国外的网络，速度很慢，所以，淘宝帮我们在国内创建了一个NPM包托管网站，能够提升使用NPM装包时候的速度！

## 配置 npm 的国内淘宝镜像

```js
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
```

## 切换镜像源

```js
npm i nrm
nrm ls
nrm use 源的名字
```

# 在Node中执行相关的JS代码的两种方式

## 进入REPL运行环境

​	直接在命令行中输入`node`,进入Node的`REPL`运行环境

| Raed     | 取用户输入的字符串内容           |
| -------- | --------------------- |
| Evaluate | 把用户输入的字符串，当作JS代码去解析执行 |
| Print    | 打印输出Evaluate解析的结果     |
| Loop     | 进入下一次循环               |

## node  要执行的JS文件路径

​	将Node代码写入到一个js文件中，然后通过`node 要执行的JS文件路径`去运行Node代码

# 使用fs模块来操作文件

## fs - 文件系统描述

> `fs` 模块提供了一些 API，用于以一种类似标准 POSIX 函数的方式与文件系统进行交互。

**写法**

```js
const fs = require('fs');
```

> 所有的文件系统操作都有异步和同步两种形式。
>
> 异步形式的最后一个参数都是完成时回调函数。 传给回调函数的参数取决于具体方法，但回调函数的第一个参数都会保留给异常。 如果操作成功完成，则第一个参数会是 `null` 或 `undefined`。

## fs.readFile

```js
// 引入核心模块
// require:关键字
// 'fs':是核心模块的名称--写死不能改
//操作文本模块
var fs = require('fs');
fs.readFile("text.txt",function(err,data){
    if (err) {
        console.log('写入失败');
    } else {
        console.log(data.toString());
    }
});
```

## fs.writeFile

```js
// 1、引入fs
var fs = require('fs')
// 2、调用写入方法
fs.writeFile("./text2.txt",'我们要追加什么呢',function (err) {
    if (err) {
        console.log('写入失败')
    } else {
        console.log('ok')
    }
});
```

## fs.appendFile

```js
var fs = require('fs')
fs.appendFile('text.txt','\n科比布莱恩特是湖人名宿',function (err) {
    if (err) {
        console.log("追加失败");
    } else {
        console.log("ok");
    }
})
```

# Node中的使用http模块创建最基本的web服务器

```js
//1、获取协议
var http = require('http');
//2、创建服务器
var server = http.createServer();
//3、开启HTTP服务器监听连接。
server.listen('3000',function () {
    console.log('http://127.0.0.1:3000');
});
//4、监听用户请求
/* 用户发送请求都会触发一个事件：request
    req：发送请求时传递过来的请求报文
    res：响应时的响应报文
*/
server.on('request',function(req,res) {
    // end函数可以将数据（字符串）返回到客户端
    // res.end('湖人总冠军');
    // 返回中文
    // 设置响应头：Content-Type
    res.writeHead(200,{
        'Content-Type':'text/html;charset=utf-8'
    });
    // 一个完整的响应必须有end
    res.end('返回中文');
});
```

![](/mdImg/构建node服务器1.png)

# 根据用户请求的路径返回不同的内容

```js
// 1、获取协议
var http = require('http');
// 2、创建一个服务器
var server = http.Server();
// 3、添加对请求端口的监听(设置所有监听的端口)
server.listen('3000',function() {
    console.log('http://127.0.0.1:3000');
});
//4、监听用户的请求
server.on('request',function(req,res) {
    //接收用户请求路径：是基于服务器根目录的路径
    var url = req.url;
    console.log(url);
    // 返回中文
    // 设置响应头：Content-Type
    res.writeHead(200,{
        'Content-Type':'text/html;charset=utf-8'
    });
    //实现业务处理
    if (url == '/index' || url == '/') {
        res.end('首页');
    } else if (url === '/category') {
        res.end('分类页面')
    } else if (url === '/cart') {
        res.end('购物车页面')
    } else {
        res.end('404')
    }
});
```

![](/mdImg/构建node服务器2.png)

# 根据用户请求的路径返回不同的页面内容

```js
// 5、监听用户的请求
server.on('request',function(req,res) {
    //接收用户请求的路径：是基于服务器根目录的路径
    var url = req.url;
    //实现业务逻辑
    if (url === '/index' || url === '/') {
        fs.readFile('./page/index.html',function(err,data) {
            if (err) res.end('err');
            else {
                res.end(data.toString());
              	/*toString()会自动编码*/
            }
        });
    }else {
        res.end('404')
    }
});
```

# 在响应不同页面的同时响应静态资源（css、图片、js）

```js
// 5、监听用户的请求
server.on('request',function(req,res) {
    // 接收用户请求的路径：是基于服务器根目录的路径
    var url = req.url;
    // 实现业务处理
    if (url === '/index' || url === '/') {
        fs.readFile('./page/index.html',function(err,data) {
            if (err) res.end('err');
            else {
                res.end(data.toString());
            }
        });
    } else if (url === '/css/index.css') {
        fs.readFile('./css/index.css',function(err,data) {
            if (err) {
                res.end('err');
            } else {
                res.end(data.toString());
            }
        });
    }   
});
```

# **升级**4：根据不同URL地址 - 响应不同文件的改造

```js
// 5、监听用户的请求
server.on('request',function(req,res) {
    //接收用户请求路径：是基于服务器根目录的路径
    var url = req.url;
    // console.log(__dirname);
    //业务实现
    /* __dirname:
    相当于当前服务器的根目录：
    当前你以哪个文件作为运行服务器的目录--这个目录就是根目录
    */
    fs.readFile(__dirname+url,function(err,data) {
        if (err) res.end('err')
        else {
            res.end(data);
        }
    });
});
```

![](/mdImg/构建node服务器3.png)