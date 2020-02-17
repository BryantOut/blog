---
title: P4_TypeScript中的类 ES5中的类和静态方法 继承
tags: TypeScript
categories: TypeScript
date: 2020-02-18
---

[TS入门教程](https://ts.xcatliu.com/ )

# P4_TypeScript中的类 ES5中的类和静态方法 继承

[面向对象编程](https://www.liaoxuefeng.com/wiki/1022910821149312/1023022126220448)

## es5 声明一个类

```js
function Person() {
    this.name = "张三";
    this.age = 20;
    this.run = function() {
        alert(this.name+'在运动');
    }
}

Person.prototype.sex = "男";
Person.prototype.work = function() {
    alert(this.name + "在工作");
} 

var p = new Person();
alert(p.name);
```

<!--more-->

## es5 声明静态方法

> **静态方法**就是不用new对象直接调用

```js
function Person() {
    this.name = "张三";
    this.age = 20;
    this.run = function() {
        alert(this.name+'在运动');
    }
}

Person.getInfo = function() {
   	console.log("我是静态方法");
}
// 调用静态方法
Person.getInfo();
```

## es5 里面的继承（不包括原型链）

> web 类继承Person类，原型链+对象冒充的组合继承模式

```js
function Person() {
    this.name = "张三";
    this.age = 20;
    this.run = function() {
        alert(this.name+'在运动');
    }
}

Person.prototype.sex = "男";
Person.prototype.work = function() {
    alert(this.name + "在工作");
}

function Web() {
    Person.call(this); // 对象冒充实现继承
}

var w = new Web();
w.run(); // 对象冒充可以继承构造函数里面的属性和方法
w.work(); // 错误！！对象冒充可以继承构造函数里面的属性和方法，但是没法继承原型链上面的属性和方法
```

## es5 里面的继承（包括原型链继承）

> 原型链实现继承，可以继承构造函数里面的属性和方法，也可以继承原型链上面的属性和方法。

```js
function Person() {
    this.name = "张三";
    this.age = 20;
    this.run = function() {
        alert(this.name+'在运动');
    }
}

Person.prototype.sex = "男";
Person.prototype.work = function() {
    alert(this.name + "在工作");
}

function Web() {
    
}

Web.prototype = new Person(); // 原型链实现继承

var w = new Web();
w.work(); // 正确
```

**问题及缺点**

```js
function Person(name,age) {
    this.name = name;
    this.age = age;
    this.run = function() {
        alert(this.name+'在运动');
    }
}

Person.prototype.sex = "男";
Person.prototype.work = function() {
    alert(this.name + "在工作");
}

function Web() {
    
}

Web.prototype = new Person(); // 原型链实现继承

var w = new Web("唠嗑"，41); 
w.work(); // 错误，实例化字类不能给父类赋值传参--"undefined在工作"
```

## 原型链+构造函数的组合继承模式

```js
 function Person(name, age) {
    this.name = name;
    this.age = age;
    this.run = function() {
        console.log(this.name + "在跑步");
    }
}

Person.prototype.work = function() {
    console.log(this.name + "在工作");
}

function Kobe(name, age) {
    Person.call(this, name, age); // 对象冒充继承，实例化子类可以给父类传参
}

Kobe.prototype = new Person(); // 原型链实现继承

var mamba = new Kobe("科比布莱恩特", 41);

mamba.work();
```

## 原型链+对象冒充继承的另一种方式

```js
 function Person(name, age) {
    this.name = name;
    this.age = age;
    this.run = function() {
        console.log(this.name + "在跑步");
    }
}

Person.prototype.work = function() {
    console.log(this.name + "在工作");
}

function Kobe(name, age) {
    Person.call(this, name, age); // 对象冒充继承，实例化子类可以给父类传参
}

Kobe.prototype = Person.prototype; // 原型链实现继承

var mamba = new Kobe("科比布莱恩特", 41);

mamba.work();
```

