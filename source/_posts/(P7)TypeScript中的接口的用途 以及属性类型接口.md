---
title: P7_TypeScript中的接口的用途 以及属性类型接口
tags: TypeScript
categories: TypeScript
date: 2020-02-18
---

[TS入门教程](https://ts.xcatliu.com/ )

# P7_TypeScript中的接口的用途 以及属性类型接口

> 接口的作用：在面向对象的编程中，接口是一种规范的定义，它定义了行为和动作的规范，在程序设计里面，接口起到一种限制和规范的作用。接口定义了某一批类所需要遵守的规范，接口不关心这些类的内部状态数据，也不关心这些类里面方法的实现细节，它只规定这批类里面必须提供某些方法，提供这些方法的类就可以满足实际需要。typescipt中的接口类似于java，同时还增加了更灵活的接口类型，包括属性、函数、可索引和类的等。

## ts中定义方法

```typescript
function methods1():void {
    console.log("methods1");
}
methods1();
```

<!--more-->

## ts中定义方法传入的参数

```typescript
function methods1(parameter1:string):void {
    console.log("methods1");
}
methods1("字符串");
```

## ts中自定义方法传入参数对 json 进行约束

```typescript
function methods1(info:{flag:string}):vpid {
    console.log("methods1")
}

methods1("curry")；/* 错误写法 */
methods1({name:"curry"}); /* 错误写法 */
methods1({flag:"curry"}); /* 正确写法 */
```

## ts中定义接口

> 接口：行为和动作的规范，对批量方法进行约束

### 属性接口

> 传入参数必须包含 firstName，secondName

```typescript
interface FullName{
    firstName:string;
    secondName:string；
}

function printName(name:FullName) {
    // 必须传入对象 firstName,secondName
    console.log(name.firstName+"---"+name.secondName);
}

printName("curry"); // 错误写法

var obj = {
    firstName: Stephen,
    secondName: Curry,
    number:30
}

printName(obj); // 正确写法
```

> 可选属性

```typescript
interface FullName{
    firstName:string;
    secondName?:string；
}

function printName(name:FullName) {
    // 必须传入对象 firstName,secondName
    console.log(name.firstName+"---"+name.secondName);
}

var obj = {
    firstName: Stephen,
    number:30
}

printName(obj); // 正确写法
```



