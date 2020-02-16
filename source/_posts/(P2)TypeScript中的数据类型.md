---
title: P2_TypeScript中的数据类型
tags: TypeScript
categories: TypeScript
date: 2020-02-17
---

[TS入门教程](https://ts.xcatliu.com/ )

# P2 TypeScript 中的数据类型

> TypeScript 中为了使编写的代码更规范，更有利于维护，增加了类型校验，在 TypeScript 中主要给我们提供了以下数据类型。

- 布尔类型（boolean）
- 数字类型（number）
- 字符串类型（string）
- 数组类型（array）

```js
// 指定数组每一项的数据类型
var arr:number[] = [11,12,13];
var arr:Array<number> = [11,12,13];
```

- 元组类型（tuple）

> 属于数组中的一种，**给数组每一个位置指定类型**

```js
var arr[number,strinig] = [123,"he is kobe"];
```

<!--more-->

- 枚举类型（enum）

> 随着计算机的不断普及，程序不仅只用于数值计算，还更广泛地用于处理非数值的数据。
>
> 例如：性别、月份、星期几、颜色、单位名、学历、职业等，都不是数值数据。
>
> 在其他程序设计语言中，一般用一个数值来代替某一个状态，这种处理方法不直观，易读性差。
>
> 如果能在程序中用自然语言中相应含义的单词来代替某一个状态，则程序就很容易阅读和理解。
>
> 也就是说，事先考虑到某一变量可能的取值，尽量用自然语言中含义清楚的单词来表示它的每一个值，这种方法称为枚举，用这个方法定义的类型称枚举类型。

```js
enum 枚举名 {
    标识符[=整形常数],
   	标识符[=整形常数],
    ……,
    标识符[=整形常数]
}

// 如果标识符没有赋值，那么打印出来就是小标

enum Sex {
    femail=0,
    mail=1
}

var mySex:Sex = Sex.femail;
console.log(mySex);
```

- 任意类型（any）

```js
var num:any = 123;
num="str";
num=true;

// 任意类型的用法
var oBox:any = document.getElementById('box');
oBox.style.color = 'red';
```

- null 和 undefined -- 其他（never类型）类型的子类型

```js
// 变量定义未赋值不报错
var num:number | undefined;
console.log(num);
```

![](/mdImg/ts6.png)

- void 类型:typescript中的void表示没有任何类型，一般用于定义方法的时候方法没有返回值。

```js
// 表示方法没有返回任何类型
function run():void {
    console.log(123)
}

// 表示返回数字类型
function run():number {
    return 123;
}
```

- never 类型

> 是其他类型（包括 null 和 undefined） 的子类型，代表从不会出现的值，这意味着声明never的变量只能被never类型所赋值。

```js
var a:undefined;
a=undefined;

var b:null;
b=null;

var c:naver;
a=123; // 错误写法；

d=(()=>{
    throw new Error('错误');
})
```

## es5中变量声明时不用指定数据类型，且可任意转换类型

```js
var flag = true;
flag = 456; /* ts中抱错 */
```

![](/mdImg/ts5.png)

