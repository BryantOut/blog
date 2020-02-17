---
title: P6_TypeScript中的类 类中的静态属性 静态方法 抽象类 多态（下）
tags: TypeScript
categories: TypeScript
date: 2020-02-18
---

[TS入门教程](https://ts.xcatliu.com/ )

# P6_TypeScript中的类 类中的静态属性 静态方法 抽象类 多态（下）

## es5 中静态方法和实例方法的区别

> 区别在于实例的需要 new 对象时候才能调用

```js
function Person(){
    this.attribute1 = "attribute1"; // 实例属性
    this.methods1 = function() {} // 实例方法
}

Person.attribute2 = "attribute2"; // 静态属性
Person.methods2 = function() {} // 静态方法
```

<!--more-->

## ts中定义静态方法

```typescript
class Person {
    public name:string;
    // 静态属性
    static sex = "男";
    constructor(name:string) {
        this.name = name;
    }
    methods1() {
        console.log('实例方法');
    }
    static methods2() {
        console.log('静态方法'); // 静态方法里面没法直接调用类里面的属性，调用的话将属性也改为静态属性
    }
}

Person.methods2();
alert(Person.sex);
```

## 多态（属于继承）

> 父类定义一个方法不去实现，让继承它的子类去实现，每一个子类有不同的表现

```typescript
class Animal{
    public name:string;
    constructor(name:string) {
        this.name = name;
    }
    eat(){}
}

class Dog extends Animal{
    constructor(name:string) {
        super(name);
    }
    eat():string {
        return `${this.name}吃骨头`;
    }    
}

class Cat extends Animal{
    constructor(name:string) {
        super(name);
    }
    eat():string {
        return `${this.name}吃鱼`;
    }    
}
```

## ts中的抽象类（它是提供其他类继承的基类，不能直接被实例化）

> 用abstract关键字定义抽象类和抽象方法，抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。

abstract抽象方法只能放在抽象类中

```js
// 抽象类和抽象方法用来定义标准， 标准：Animal 这个类要求它的子类必须包含eat方法
abstract class Animal{
    public name:string;
    constructor(name:string) {
        this.name = name;
    }
    abstract eat():any;
}

class Cat extends Animal{
    constructor(name:string) {
        super(name);
    }
    // eat():string {
    //     return `${this.name}吃鱼`;
    // }    
}
```

![](/mdImg/ts9.png)

```ty
// 不能直接被实例化
abstract class Animal{
    public name:string;
    constructor(name:string) {
        this.name = name;
    }
    abstract eat():any;
}

var Dog = new Animal("狗");
```

![](/mdImg/ts10.png)