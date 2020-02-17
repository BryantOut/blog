---
title: P3_TypeScript中的函数完整版
tags: TypeScript
categories: TypeScript
date: 2020-02-17
---

[TS入门教程](https://ts.xcatliu.com/ )

# P3_TypeScript中的函数完整版

## es5 函数定义方法

> 函数声明法

```js
function run() {
    return "run"；
} 
```

> 匿名函数

```js
var run2 = function() {
    return "run"；
}
```

<!--more-->

## ts声明函数

> 返回值也要指定类型

```js
// 函数声明法
function run():string {
    return 'run'；
}

// 匿名函数
var run2 = function():number {
    return 123；
}

// 错误写法
function run():string {
    return 123；
}
```

## 参数需要指定类型

```js
function getInfo(name:string,age:number):string {
    return `${name} --- ${age}`；
}
```

## 没有返回值的方法

```js
function run():void {
    console.log('run')；
}
```

## 方法可选参数

```js
function getInfo(name:string,age?:number):string {
    if (age) {
        return `${name} --- ${age}`；
    } else {
        return `${name} --- 年龄保密`；
    }
}
```

## 默认参数

> **es5里面没法设置默认参数，es6和ts中都可以设置默认参数**

```js
function getInfo(name:string,age?:number=20):string {
    if (age) {
        return `${name} --- ${age}`；
    } else {
        return `${name} --- 年龄保密`；
    }
}

console.log(getInfo('张三')); // 张三 --- 20
```

## 剩余参数

```js
function sum(a:number,b:number,c:number,d:number):number {
    return a+b+c+d;
}
```

> 三点运算符，接收新参数传过来

```js
function sum(...result:number[]):number {
    var sum = 0;
    for(var i=0;i<result.length;i++) {
        sum+result[i];
    }
    return sum;
}
```

## 函数重载

> java 中方法的重载：重载指的是两个或者两个以上同名函数，但它们的参数不一样，这时会出现函数重载的情况

> typescript 中的重载：通过为同一个函数提供多个函数类型定义来设下多种功能的目的。

**ts为了兼容 es5 和 es6 重载的写法和 java 中有区别**

```js
// es5 中出现同名的方法，下面的会替换上面的方法
function css(config) {
    
}

function css(config,value) {
    
}

// ts中的重载
function getInfo(name:string):string;

function getInfo(age:number):number;

function getInfo(str:any):any {
    if (typeof str === 'string') {
        return '我叫：'+ str;
    } else {
        return '我的年龄是' + str;
    }
}

alert(getInfo('张三'));
alert(getInfo(20));
alert(getInfo(true)); // 错误
```

## 箭头函数

[相关文档](https://www.liaoxuefeng.com/wiki/1022910821149312/1031549578462080 )