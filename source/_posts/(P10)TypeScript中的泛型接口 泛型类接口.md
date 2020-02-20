---
title: P10_TypeScript中的泛型接口 泛型类接口
tags: TypeScript
categories: TypeScript
date: 2020-02-20
---

[TS入门教程](https://ts.xcatliu.com/ )

## P10_TypeScript中的泛型接口 泛型类接口 

## 泛型

- 软件工程中，我们不仅要创建一致的定义好的良好API，同时也要考虑可重用性。组建不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵魂的功能

<!--more-->

- 在像 **C#** 和 **java** 这样的语言中，可以使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据。这样用户就可以根据自己的数据来使用组件。
- 通俗理解：泛型就是解决**类**、**接口**、**方法**的**复用性**，**以及对不特定数据类型的支持。**

## 回顾函数接口

```typescript
interface functionInterface {
    (value1:string,value2:string):string;
}

var myFun:functionInterface = function(value1:string,value2:string) {
    return value1 + value2;
}

alert(myFun("科比布莱恩特","24"));
```

## 改造成泛型接口

```typescript
interface functionInterface {
    <T>(value1:T,value2:T):T;
}

var myFun:functionInterface = function<T>(value1:T,value2:T):T {
    console.log(value2);
    return value1;
}

alert(myFun<string>("科比布莱恩特","24"));
```

**写法二**

```typescript
interface functionInterface<T> {
    (value1:T):T;
}

function myFun<T>(value1:T) {
    return value1;
}

var myGetData:functionInterface<string> = myFun;

alert(myGetData("史蒂芬库里"));
```

## 把类当作参数来约束数据传入的类型

```typescript
class User {
    username:string | undefined; // 表示该属性可以是 string 类型，也可以是 undefined 类型，也可以是
    number:number | undefined;
    constructor(username:string,number:number) {
        this.username = username;
        this.number = number;
    }
}

class MySqlDb {
    add(config:User):boolean {
        console.log(config);
        return true;
    }    
}

var userOne = new User("科比布莱恩特",24);
var mySqlDb = new MySqlDb();
mySqlDb.add(userOne);
```

## 把类当作参数的泛型类

```typescript
/*
	定义一个User的类，这个类的作用就是映射数据库字段。
	然后定义一个 MySqlDb 的类，这个类用于操作数据库。
	然后把 User 类作为参数传入到 MySqlDb 中
	
    var user = new User({
    	username: "Curry",
    	number: 30
    })
    
    var Db = new MySqlDb();
    Db.add(user);
*/
```

报错情况

```typescript
class User {
    username:string;
    number:number;
}
```

![](/mdImg/ts12.png)

解决

```typescript
class User {
    username:string | undefined; // 表示该属性可以是 string 类型，也可以是 undefined 类型，也可以是
    number:number | undefined;
}
```

## 操作数据库的泛型类

```typescript
class MySqlDb<T> {
    add(info:T):boolean {
        console.log(info);
        return true;
    } 
}

class User {
    username:string;
    number:number;
    constructor(username:string,number:number) {
        this.username = username;
        this.number = number;
    }
}

var userOne = new User("科比布莱恩特",24);
var mySqlDb = new MySqlDb<User>();
mySqlDb.add(userOne);
```

