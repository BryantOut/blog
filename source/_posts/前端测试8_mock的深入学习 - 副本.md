---
title: 前端测试8_mock的深入学习
tags: Jest
categories: Jest
date: 2020-04-10
---

# 需求1：测试接口

> 前端自动化测试一般不会发送真正的ajax请求

```js
// demo.js
import axios from 'axios';

export const fetchData = () => {
    return axios.get('/').then(res => res.data);
}

// 假如后端返回
// { data: "(function(){return '123'})()" }
```

## 第一种写法：`jest.mock('axios')`

> 对axios进行模拟

```js
// demo.test.js
import { fetchData } from './demo';
import axios from 'axios';

jest.mock('axios');

test('fetchData 测试', () => {
    axios.get.mockResolvedValue({
        data: "(function(){return '123'})() "
    })
    return fetchData().then(data => {
        expect(eval(data)).toEqual('123');
    })
})
```

<!--more-->

## 第二种写法，自己写一个函数去替代异步函数

![](/mdImg/test36.png)

```js
// __mock__/demo.js
// new Promise 模拟异步请求
export const fetchData = () => {
    return new Promise((resolved, reject) => {
        resolved("(function(){return '123'})()")
    })
}

// demo.test.js
jest.mock('./demo'); // 自动去找__mock__/demo.js
import { fetchData } from './demo';


test('fetchData 测试', () => {
    return fetchData().then(data => {
        expect(eval(data)).toEqual('123');
    })
})
```

# 需求2：只模拟异步函数，不模拟同步函数

## `jest.requireActual`

```js
// demo.js
import axios from 'axios';

export const fetchData = () => {
    return axios.get('/').then(res => res.data);
}

// 假如后端返回
// { data: "(function(){return '123'})()" }

export const getNumber = () => {
    return 24;
}

// __mock__/demo.js
// new Promise 模拟异步请求
export const fetchData = () => {
    return new Promise((resolved, reject) => {
        resolved("(function(){return '123'})()")
    })
}

// demo.test.js
```

