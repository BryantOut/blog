---
title: p11_TypeScript 类型、接口、类、泛型 综合使用-- TypeScript封装统一操作MySql Mongodb Mysql的底层类库
tags: TypeScript
categories: TypeScript
date: 2020-02-20
---

[TS入门教程](https://ts.xcatliu.com/ )

# p11_TypeScript 类型、接口、类、泛型 综合使用-- TypeScript封装统一操作MySql Mongodb Mysql的底层类库

```typescript
/*
    功能：定义一个操作数据库的库 支持 MySql MsSql MongoDb
    要求一：MySql MsSql MongoDb 功能一样，都有 add,update,delete,get方法
    注意：约束统一的规范、以及代码的重用

    解决方案：需要约束规范，所以要定义接口，需要代码重用所以用到泛型
        1.接口：在面向对象的编程中，接口是一种规范的定义，它定义了行为和动作的规范
        2.泛型：通俗理解，泛型就是解决，类、接口、方法的复用性
*/
```

<!--more-->

> 要实现泛型接口，这个类也必须是一个泛型类

```typescript
interface DbInterface<T> {
    add(info:T):boolean;
    update(info:T,id:number):boolean;
    delete(id:number):boolean;
    get(id:number):any[];
}

// 定义一个操作mySql数据库，注意：要实现泛型接口，这个类也应该是一个泛型类
class MySql<T> implements DbInterface<T> {
    add(info: T): boolean {
        throw new Error("Method not implemented.");
    }   
    update(info: T, id: number): boolean {
        throw new Error("Method not implemented.");
    }
    delete(id: number): boolean {
        throw new Error("Method not implemented.");
    }
    get(id: number): any[] {
        throw new Error("Method not implemented.");
    }    
}

// 定义一个操作msSql数据库
class MsSql<T> implements DbInterface<T> {
    add(info: T): boolean {
        console.log(info);
        return true;
    }    
    update(info: T, id: number): boolean {
        throw new Error("Method not implemented.");
    }
    delete(id: number): boolean {
        throw new Error("Method not implemented.");
    }
    get(id: number): any[] {
        throw new Error("Method not implemented.");
    }
}

// 定义一个操作MongoDb数据库
class MongoDb<T> implements DbInterface<T> {
    add(info: T): boolean {
        throw new Error("Method not implemented.");
    }    
    update(info: T, id: number): boolean {
        throw new Error("Method not implemented.");
    }
    delete(id: number): boolean {
        throw new Error("Method not implemented.");
    }
    get(id: number): any[] {
        throw new Error("Method not implemented.");
    }
} 


class User {
    public username:string;
    constructor(username:string) {
        this.username = username;
    }
}

var curry = new User("史蒂芬库里");
var my_msSql = new MsSql<User>();
my_msSql.add(curry);
```

