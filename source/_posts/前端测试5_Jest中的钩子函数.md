---
title: 前端测试5_Jest中的钩子函数
tags: Jest
categories: Jest
date: 2020-03-20
---

## 引出问题

> 两个测试用例需要用到同一个实例，这样测试用例之间相互之间会有影响

```js
// counter.js
 export default class Counter {
     constructor() {
         this.number = 0;
     }
     addOne() {
         this.number += 1;
     }
     minusOne() {
         this.number -= 1;
     }
 }
 
 // Counter.test.js
 import Counter from './Counter';

const counter = new Counter();

test('测试 Counter 中的 addOne 方法', () => {
    counter.addOne();
    expect(counter.number).toBe(1);
})

test('测试 Counter 中的 minusOne 方法', () => {
    counter.minusOne();
    expect(counter.number).toBe(0);
})
```

<!--more-->

## 引出钩子函数

> 在 `jest` 里面，如果在测试之前要对一些基础内容进行初始化的时候， `jest` 建议我们使用钩子函数

```js
import Counter from './Counter';

let counter = null;

beforeAll(() => {
    console.log('beforeAll')
})

beforeEach(() => {
    console.log('beforeEach')
    counter = new Counter();
})

afterAll(() => {
    console.log('afterAll')
})

afterEach(() => {
    console.log('afterEach')
})

test('测试 Counter 中的 addOne 方法', () => {
    console.log('测试 Counter 中的 addOne 方法')
    counter.addOne();
    expect(counter.number).toBe(1);
})

test('测试 Counter 中的 minusOne 方法', () => {
    console.log('测试 Counter 中的 minusOne 方法')
    counter.minusOne();
    expect(counter.number).toBe(-1);
})
```

![](/mdImg/test27.png)

## `jest` 分组语法 `describe` 及 钩子函数的作用域

```js
import Counter from './Counter';

describe('测试Counter 的测试代码 ', () => {
    let counter = null;

    beforeAll(() => {
        console.log('beforeAll')
    })

    beforeEach(() => {
        console.log('beforeEach')
        counter = new Counter();
    })

    afterAll(() => {
        console.log('afterAll')
    })

    afterEach(() => {
        console.log('afterEach')
    })

    describe('测试增加相关的代码', () => {
        test('测试 Counter 中的 addOne 方法', () => {
            console.log('测试 Counter 中的 addOne 方法')
            counter.addOne();
            expect(counter.number).toBe(1);
        })

        test('测试 Counter 中的 addTwo 方法', () => {
            console.log('测试 Counter 中的 addTwo 方法')
            counter.addTwo();
            expect(counter.number).toBe(2);
        })
    })

    describe('测试减少相关的代码', () => {
        test('测试 Counter 中的 minusOne 方法', () => {
            console.log('测试 Counter 中的 minusOne 方法')
            counter.minusOne();
            expect(counter.number).toBe(-1);
        })

        test('测试 Counter 中的 minusTwo 方法', () => {
            console.log('测试 Counter 中的 minusTwo 方法')
            counter.minusTwo();
            expect(counter.number).toBe(-2);
        })
    })
})
```

![](/mdImg/test28.png)

