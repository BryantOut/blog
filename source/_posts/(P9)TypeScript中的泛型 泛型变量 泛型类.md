---
title: P9_TypeScript中的泛型 泛型变量 泛型类
tags: TypeScript
categories: TypeScript
date: 2020-02-20
---

[TS入门教程](https://ts.xcatliu.com/ )

## P9_TypeScript中的泛型 泛型变量 泛型类 

## 泛型

- 软件工程中，我们不仅要创建一致的定义好的良好API，同时也要考虑可重用性。组建不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵魂的功能

<!--more-->

- 在像 **C#** 和 **java** 这样的语言中，可以使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据。这样用户就可以根据自己的数据来使用组件。
- 通俗理解：泛型就是解决**类**、**接口**、**方法**的**复用性**，**以及对不特定数据类型的支持。**

```typescript
// 泛型: 可以支持不特定的数据类型，要求：传入的参数和返回的参数数据类型一致
// T表示泛型，具体什么类型是调用这个方法的时候决定的

function getData<T>(value:T):T {
    return value;
}

getData<number>(123);

getData<string>("123");

// getData<string>(123); // error
```

![](/mdImg/ts11.png)

## 泛型类

```typescript
class MInClass<T> {
    public list:T[] = [];
    add(value:T) {
        this.list.push(value);
    }
    min():T {
        var minNum = this.list[0];
        for(let i=0;i<this.list.length;i++) {
            if (this.list[i+1]<this.list[i]) {
                minNum = this.list[i+1];
            }
        }
        return minNum;
    }
}

var m = new MInClass<number>();
m.add(12);
m.add(1);
m.add(2);
m.add(0);
alert(m.min());

var m2 = new MInClass<string>();
m2.add("z");
m2.add("a");
m2.add("0");
m2.add("9");
alert(m2.min());
```

