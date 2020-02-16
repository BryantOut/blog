---
title: es6新语法的介绍
tags: es6新语法的介绍
categories: es6新特性
date: 2018-8-26
---

# ES6是什么

- **2015年6月正式发布**
- **使用babel语法转换器，支持低端浏览器**
- **流行的库基本都基于ES6构建，React，vue默认使用ES6新语法开发**

# ES6里都有什么

- **块级作用域、字符串、函数**
- **对象扩展、解构**
- **类、模块化**

# 反引号

1. **可以解析变量**
2. **可以创建多行文本**

```js
【示例1】
// 创建查询语句
var sql = 
    `insert into heros values(null,'${newObj.name}','${newObj.gender}','${newObj.img}',default)`
```

<!--more-->

3. **告别+拼接字符串**

![](/mdImg/2es6.png)

# let && const

- **定义变量使用let替代var**
- **const定义不可修改的变量**
- **作用域和{}**

![](/mdImg/1es6.png)

>  let命令也用于声明对象，但是作用域为局部。【块级作用域】

```js
【示例1】
{
    let a = 10;
    var b = 1;
}
/*
let:创建变量:
	1.变量有作用域：从创建这个变量开始，到这个变量所在的结构的}结束
	2.没有所谓的变量声名提升
*/
```

[在函数外部可以获取到b，获取不到a，因此例如for循环计数器就适合使用let。]

> const用于声明一个常量，设定后值不会再改变。

```js
【示例1】
const PI = 3.1415;
PI // 3.1415
PI = 3;
/*
const:定义变量:
	1.一般名称使用大写
	2.一旦赋值就不能再更改,如果更改就会报错：Assignment to constant variable，只有在声明的时候赋值才可以
*/
```

# 解构赋值

## 函数也可以多返回值

- **数组结构**
- **对象结构**

![](/mdImg/5es6.png)

> ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。 
> 例如数组：

```JS
【示例1】
let [a, b, c, d] = [1, 2, 3];
//等同于
let a = 1;
let b = 2;
let c = 3;
let d = undefined;
/*
解构:
  1.就是从数组或对象中提取数据并赋值
  2.数组解构：顺序对应，否则是undefined
  3.数组解构：顺序对应，否则是undefined
*/
```

```js
【示例2】
// 对象解构
var obj = {
    name: 'jack',
    // age: '20',
    gender: 'true',
    hobby: ['写代码', '调bug'],
    computer: {
        price: 10000,
        brand: 'dell'
    }
}

function fn({ name, gender, age, hobby }) {
    console.log(name)
    console.log(gender)
    if (!age) {
        age = 100
    }
    console.log(age)
}

fn(obj)

//var { name, gender, age, hobby } = obj
```

# 延展操作符

>  通过它可以将数组作为参数直接传入函数：

```js
【示例1】
var people=['Wayou','John','Sherlock'];
function sayHello(people1,people2,people3){
    console.log(
      `Hello ${people1},${people2},${people3}`);
}
//改写为
sayHello(...people);	//输出：Hello Wayou,John,Sherlock 
```

```js
【示例2】
var arr = [1,2,3]

var arr2 = ['a','b','c','d']

var temp = [...arr,...arr2]
console.log(temp)	//输出：1,2,3,a,b,c,d
```

# promise

> 引入 promise 【需求：我需要读取三个文件，同时要求读取的顺序和结果顺序一致】

```js
//【原本的做法】
var fs = require('fs')

var fs = require('fs')

fs.readFile(__dirname + "/views/a.txt", (err, data) => {
    if (err) {
        console.log('err')
    } else {
        console.log(data.toString())

        fs.readFile(__dirname + "/views/b.txt", (err, data) => {
            if (err) {
                console.log('err')
            } else {
                console.log(data.toString())

                fs.readFile(__dirname + "/views/c.txt", (err, data) => {
                    if (err) {
                        console.log('err')
                    } else {
                        console.log(data.toString())
                    }
                })
            }
        })
    }
})   
```

> 原始的做法可以解决这个需求，但是嵌套太难看

```js
var p1 = retPro("/views/a.txt")
var p2 = retPro("/views/b.txt")
var p3 = retPro("/views/c.txt")
// promise:我们创建promise对象(不是一个函数),通过Promise
// 创建的时候，需要传入一个回调函数，这个回调函数有两个参数，
// resolve：当执行成功的时候调用的回调
// reject：当执行失败的时候调用的回调
function retPro(filename){
    return new Promise((resolve,reject)=>{
        fs.readFile(__dirname + filename, (err, data) => {
            if (err) {
                reject(err)
            } else {
               resolve(data)
            }
        })
    })
}
```

```js
// 执行promise
// 一定要接收这种写法：
// 通过then来执行promise对象中的回调
p1.then((data)=>{
    console.log(data.toString())
    // 当执行完第一个回调函数之后。会继续执行return的promise对象
    return p2
}).then((data)=>{
    console.log(data.toString())
    return p3
}).then((data)=>{
    console.log(data.toString())
})
// 如果有错误，可以统一的通过catch来捕获
.catch((err) => {
    console.log('123')
})
```

# 对象扩展（Object扩展）

- **Object.keys、values、entries**
- **对象方法简写，计算属性**
- **展开运算符（不是ES6标准，但是bable也支持）**

## 旧写法

![](/mdImg/3es6.png)

## 新写法

![](/mdImg/4es6.png)

# 模块化

>  ES6中自带了模块化机制，告别sea.js和require.js

- **Import、import{}**
- **Export、Export default**
- **Node现在还不支持，还需要用require来加载文件**

## 例一：

声明，输出

![](/mdImg/6es6.png)

引用，输入

![](/mdImg/7es6.png)

## 例二：

声明，输出

![](/mdImg/8es6.png)

引用，输入

![](/mdImg/9es6.png)

#  其他

> 还有一些特性，虽然不在ES6的范围，但是也被babel支持，普遍被大家接受和使用（需要安装插件）

- **对象扩展符**
- **装饰符**
- **Async await(ES7)**