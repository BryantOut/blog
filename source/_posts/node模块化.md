---
title: node模块化
tags: node
categories: node
date: 2018-8-19
---

# 什么是程序开发中的模块及好处

1. 什么是程序开发中的模块化：把一些功能类似的代码，封装到一个单独的文件中去，这些单独抽离出来的代码文件，就能够提供各种各样好用的功能；这种通过代码功能分割文件的方式，叫做程序中的模块化；
2. 好处：保证了每个文件的功能（职能）单一；需要什么特定的功能，直接调用某一个特定的模块；对将来程序开发和维护都有好处！

>  **node由三部分组成：ECMAScript + 核心API + 第三方等API**

<!--more-->

## 核心模块

1. **什么是核心模块：**官方，发现一些功能模块使用非常频繁，然后，官方把这些模块，编译成了二进制可执行的文件，然后打包到了Node的安装包中；所以，这些核心模块就已经随着安装Node的时候，被安装到了本地；

2. 如何使用核心模块

   使用require(‘核心模块的名称’);

```js
var fs = require('fs')
```

## 第三方模块

1. **什么是第三方模块：**出了官方提供的好用的核心模块之外，我们程序员发现，还有一些使用也很频繁的代码和方法，一些牛逼的团体、个人、公司，开发出了好用的模块，通过NPM官网，托管出去，供其他人下载使用的这些模块；统称为第三方模块；
2. 如何使用第三方模块（通过使用 `moment` 第三方模块进行介绍）

![](/mdImg/node模块化1.png)

```js
/*先使用npm下载这个模块！【注意：在安装第三方模块的时候，
安装的名字，就是你在require时候导入的名字】*/
var moment = require('moment')
/*通过官方文档，试着去使用这个第三方模块！*/
var moment = moment().format('YYYY-MM-DD HH:mm:ss')
console.log(moment)
/*注意：无论是核心模块、还是第三方模块，都是通过 标识符名称来引用这个模块的！*/
```

## 用户模块

1. **什么是用户模块：**程序员自己定义的JS文件，统统属于用户模块
2. **用户模块向外导出成员的两种方式：**

### global

相当于浏览器中的window对象

**缺点**

1. 全局变量污染
2. 不知道成员是从哪个模块中暴露出去的（比如当函数名一样时）

**js代码**

```js
obj = {
    name: "科比布莱恩特",
    number: 24,
    sayHi: sayHi
}

function sayHi() {
    console.log(obj.name + ":" + obj.number);
}

global.sayHi = sayHi;
```

**html代码**

```html
require('./1用户模块中的global');

//console.log(global);
// global.sayHi.call(global.obj);
global.sayHi();
```

### exports

1. 每一个模块都有一个单独的exports
2. 它是一个对象,当这个模块被引入的时候，这个对象会自动的返回

**js代码**

```js
var obj = {
    name:"史蒂芬库里",
    num:"30",
    sayHi:function(){
        return ()=>{
            console.log("Nothing is impossible");
        }
    }
}
exports.sayHi = obj.sayHi();
```

**html代码**

```html
var firstEx = require('./用户模块中的exports')
console.log(firstEx.sayHi())
```

### exports 和 module.exports 的区别

![](/mdImg/node模块化2.png)

注意： 在一个 module 中，最终向外暴露的成员，以 module.exports 指向的对象

![](/mdImg/node模块化3.png)

## 第三方模块查找规则

1. node首先，查看项目根目录中有没有 `node_modules` 文件夹
2. 查找 `node_modules` 文件夹中，有没有和第三方模块名称一致的文件夹
3. 在模块对应的文件夹中，查找有没有 `package.json` 这个文件
4. 在 `package.json` 文件中，查找有没有 `main` 属性
5. 如果有 `main` 属性，并且 `main` 属性指向的路径存在，那么就尝试加载这个路径指定的文件！
6. 如果 `package.json` 文件中，没有 `main` 属性，或者 `main` 属性指向的路径不存在，或者没有`package.json` 文件， 那么，Node尝试加载 模块根目录中 `index` 相关文件：`index.js` -> `index.json` -> `index.node`
7. 如果在`node_modules`文件夹中，找不到对应的模块文件夹，或者在项目根目录中根本没有`node_modules`文件夹，则向上一层文件夹中去查找，查找规则同上！
8. 如果上一层目录中也没有查找到，则再向上翻一层去查找，直到找到当前项目所在的盘符根目录为止！
9. 如果找到了盘符根目录还找不到，则报错：`cannot find module ***`