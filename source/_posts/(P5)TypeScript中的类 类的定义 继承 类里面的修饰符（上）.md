---
title: P5_TypeScript中的类 类的定义 继承 类里面的修饰符（上）
tags: TypeScript
categories: TypeScript
date: 2020-02-18
---

[TS入门教程](https://ts.xcatliu.com/ )

# P5_TypeScript中的类 类的定义 继承 类里面的修饰符（上）

[面向对象编程](https://www.liaoxuefeng.com/wiki/1022910821149312/1023022126220448)

## ts中类的定义

```js
class Person{
    name:string; // 属性，前面省略了public关键词
    constructor(name:string) { // 构造函数 实例化类的时候触发的方法
		this.name = name; 
    }
    work():void {
        console.log(this.name + "在工作");
    }
}

var mamba = new Person('科比布莱恩特');
mamba.work();
```

<!--more-->

> 例2

```js
class Person{
    name:string;
    constructor(name:string) {
        this.name = name;
    }
    getName():string {
        return this.name;
    }
    setName(name:string):void {
        this.name = name;        
    }
}

var curry = new Person("史蒂芬库里");
alert(curry.getName());
```

## ts中实现继承 extends、super

```typescript
class Person {
    name:string;
    constructor(name:string) {
        this.name = name;
    }
    getName():string {
        return this.name;
    }
}

class Male extends Person {
    constructor(name:string) {
        super(name); // 表示调用父类的构造函数
    }
    work():void {
        alert(`${this.name}在工作`);
    }
}

var curry = new Male("史蒂芬库里");
alert(curry.getName());
```

## 类里面的修饰符

> ts里面定义属性的时候给我们提供了三种修饰符

| 修饰符名称            | 描述                                         |
| --------------------- | -------------------------------------------- |
| public（公有）        | 在类里面、子类、类外面都可以访问             |
| protected（保护类型） | 在类里面、子类里面可以访问，在类外面没法访问 |
| private（私有）       | 在类里面可以访问，子类、类外部都没法访问     |

属性如果不加修饰符，默认就是 **piblic(公有)**

> public

```js
class Person{
    public name:string;
    constructor(name:string) {
        this.name = name;
    }
    getName():string {
        return this.name;
    }
}

var curry = new Person("史蒂芬库里");
alert(curry.name);
```

> protected

```typescript
class Person{
    protected name:string;
    constructor(name:string) {
        this.name = name;
    }
    getName():string {
        return this.name;
    }
}

var curry = new Person("史蒂芬库里");
alert(curry.name); // error
```

![](/mdImg/ts7.png)

> private

```typescript
class Person{
    private name:string;
    constructor(name:string) {
        this.name = name;
    }
    getName():string {
        return this.name;
    }
}

class Male extends Person{
    constructor(name:string) {
        super(name);
    }

    setName():void {
        this.name = "克莱汤普森"; // error
    }
}

var curry = new Male("史蒂芬库里");
```

![](/mdImg/ts8.png)