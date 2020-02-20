---
title: P8_TypeScript中的接口类型
tags: TypeScript
categories: TypeScript
date: 2020-02-20
---

[TS入门教程](https://ts.xcatliu.com/ )

## 属性类型接口 

> jq

```js
$ajax({
    type: "GET",
    url: "test.json",
    data: {username:$("#username").val},
    dataType: "json"
})
```

<!--more-->

> ts自己封装

```typescript
interface Config {
    type: string;
    url: string;
    data?: string;
    dataType: string;
}

function ajax(config:Config) {
    var xhr = new XMLHttpRequest();
    
    xhr.open(config.url,config.url,true);
    
    xhr.send(config.data);
    
    xhr.omreadystatechange = function() {
        if (xhr.readyState === 4 && xhr.status === 200) {
        	console.log("success");
            if (config.dataType === "json") {
                JSON.parse(xhr.responseText);
            } else {
                console.log(xhr.responseText)
            }
        }
    }
}

ajax({
    type: "get",
    url: "http://www.baidu.com",
    dataType: "json"
})
```

## 函数类型接口

> 对方法传入的参数以及返回值进行约束

```typescript
interface methodsRule {
    (key:string,value:string):string;
}

var methods:methodsRule = function(key:string,value:string):string {
    // 模拟
    return key+value;
}
console.log(methods("name","curry"))
```

## 可索引接口

> 数组、对象的约束（不常用）

ts中定义数组的方式

```typescript
var arr:number[] = [30,24];
var arr1:Array<string> = ["30","24"];
```

ts针对**数组**的接口

```typescript
interface UserArr {
    [index:number]:string
    // 索引值必须是number类型，value值必须是string类型
}

var arr:UserArr = ['30','24'];
```

ts针对**对象**的接口

```typescript
interface UserObj {
    [index:string]:string
}

var arr:UserObj = {name:'20'};
```

## 类类型接口

> 对类的约束，和**抽象类**有点相似

```typescript
interface ClassInterface {
    name: string;
    eat():void;
}

class Dog implements ClassInterface {
    name:string;
    constructor(name:string) {
        this.name = name;
    }
    eat() {
        alert(this.name);
    }
}

var DogOne = new Dog("小黑");
DogOne.eat();
```

## 接口扩展

```typescript
interface Animal {
    eat():void;
}

interface People extends Animal {
    work():void;
}

class Curry implements People {
    eat() {}
    work() {}
}
```

## 接口扩展结合接口继承

```typescript
interface Animal {
    eat():void;
}

interface People extends Animal {
    work():void;
}

class Male implements People {
    name: string;
    constructor(name:string) {
        this.name = name;
    }
    eat(){
        alert(`${this.name}在吃饭`);
    };
    work(){
        alert(`${this.name}在工作`);
    };
}

class Curry extends Male {
    name:string;
    constructor(name:string) {
        super(name);
        this.name = name;
    }
    work(){
        alert(`${this.name}在打NBA`);
    };
}

var curry = new Curry("史蒂芬库里");
curry.work();
```

