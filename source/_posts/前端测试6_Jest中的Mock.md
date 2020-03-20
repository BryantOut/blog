---
title: 前端测试6_Jest中的Mock
tags: Jest
categories: Jest
date: 2020-03-20
---

## 原本繁琐的例子

```js
// demo.js
export const runCallback = (callback) => {
    return callback();
}

// demo.test.js
import { runCallback } from './demo';

test('测试 runCallback', () => {
    const func = () => {
        return 'hello';
    }
    expect(runCallback(func).toBe('hello')) // 断言
})
```

<!--more-->

## 借助 `jest` 提供的 `mock` 解决问题

```js
// demo.js
export const runCallback = (callback) => {
    callback();
}

// demo.test.js
import { runCallback } from './demo';

test('测试 runCallback', () => {
    const func = jest.fn(); // moock 函数，捕获函数的调用
    runCallback(func);
    expect(func).toBeCalled()
})
```

# mock的参数

## 捕获函数的调用和返回结果，以及this和调用顺序

### calls

> 包含对这个模拟函数所做的所有调用的调用参数的数组。数组中的每个项都是在调用期间传递的参数数组

```js
// demo.js
export const runCallback = (callback) => {
    callback('湖人总冠军');
}

// demo.test.js
import { runCallback } from './demo';

test('测试 runCallback', () => {
    const func = jest.fn();
    runCallback(func);
    runCallback(func);
    // expect(func).toBeCalled(); // 测试是否请求
    expect(func.mock.calls.length).toBe(2); // 测试请求次数
    expect(func.mock.calls[0]).toEqual(['湖人总冠军']); // 测试请求参数
    console.log(func.mock);
})
```

![](/mdImg/test29.png)

### 模拟参数

```js
import { runCallback } from './demo';

test('测试 runCallback', () => {
    const func = jest.fn(() => {
        return '科比布莱恩特科'
    });
    runCallback(func);
    runCallback(func);
    console.log(func.mock);
})
```

![](/mdImg/test30.png)

### instances

> 一个数组，其中包含使用new从这个模拟函数实例化的所有对象实例

```js
// demo.js
export const createObject = (classItem) => {
    new classItem();
}
// demo.test.js
test.only('createObject', () => {
    const func = jest.fn();
    createObject(func);
    console.log(func.mock)
})
```

### results 

> 被执行了多少次和返回的结果

### invocationCallOrder

> 测试执行的顺序

## 它可以让我们自己设置返回的结果--(mockReturnValueOnce)

```js
import { runCallback, createObject } from './demo';

test('测试 runCallback', () => {
    const func = jest.fn();
    func.mockReturnValueOnce('史蒂芬库里')
    func.mockReturnValueOnce('克莱汤普森')
    runCallback(func);
    runCallback(func);
    console.log(func.mock);
})

test.only('createObject', () => {
    const func = jest.fn();
    createObject(func);
    console.log(func.mock)
})
```

> ![](/mdImg/test31.png)

## 改变函数的内部实现--(mockResolvedValue)

> 模拟ajax返回结果，因为如果项目有成百上千个接口需要测试，那么需要耗费的时间太长

```js
// demo.js
import  axios  from  'axios';

export  const  getData  =   ()  =>  {    
    return  axios.get('/api').then(res  =>  res.data);
}

// demo.test.js
import { getData } from './demo';
import axios from 'axios';
jest.mock('axios');

test('测试 getData', async() => {
    axios.get.mockResolvedValue({ data: '湖人总冠军' });
    // axios.get.mockResolvedValueOnce({ data: '湖人总冠军' });
    await getData().then((data) => {
        expect(data).toBe('湖人总冠军');
    })
})
```

## 补充

### mockImplementation

```js
import { runCallback } from './demo';

test('测试 runCallback', () => {
    // const func = jest.fn(() => {
    //     return '科比布莱恩特科';
    // });
    const func = jest.fn();
    
    //func.mockImplementation(() => {
    //    console.log('湖人总冠军');
    //    return '科比布莱恩特科';
    //})
    
    func.mockImplementationOnce(() => {
        console.log('湖人总冠军');
        return '科比布莱恩特科';
    })
    
    runCallback(func);
    runCallback(func);
    console.log(func.mock);
})
```





