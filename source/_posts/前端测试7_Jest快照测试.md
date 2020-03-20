---
title: 前端测试7_Jest快照测试
tags: Jest
categories: Jest
date: 2020-03-20
---

# Snapshot 快照测试

## 原本写法

> 在 `generateConfig` 中追加新的配置的时候，断言也要修改，比较麻烦

```js
// demo.js
export const generateConfig = () => {
    return {
        server: 'http://localhost',
        port: 8080
    }
}

// demo.test.js
import { generateConfig } from './demo';

test('测试 generateConfig 函数', () => {
    expect(generateConfig()).toEqual({
        server: 'http://localhost',
        port: 8080
    })
})
```

<!--more-->

## Snapshot 写法

```js
// demo.js
export const generateConfig = () => {
    return {
        server: 'http://localhost',
        port: 8080
    }
}

// demo.test.js
import { generateConfig } from './demo';

test('测试 generateConfig 函数', () => {
    expect(generateConfig()).toMatchSnapshot();
})
```

![](/mdImg/test33.png)

### 当我们新增一个配置

```js
// demo.js
export const generateConfig = () => {
    return {
        server: 'http://localhost',
        port: 8080,
        author: 'kobe'
    }
}

// demo.test.js
import { generateConfig } from './demo';

test('测试 generateConfig 函数', () => {
    expect(generateConfig()).toMatchSnapshot();
})
```

![](/mdImg/test34.png)

![](/mdImg/test35.png)

### 当遇到快照内容是随时变化的

```js
// demo.js
export const generateConfig = () => {
    return {
        server: 'http://localhost',
        port: 8080,
        author: 'kobe',
        time: new Date()
    }
}

// demo.test.js
import { generateConfig } from './demo';

test('测试 generateConfig 函数', () => {
    expect(generateConfig()).toMatchSnapshot({
        time: expect.any(Date)
    });
})
```

## 行内Snapshot 写法

### 需要安装一个模块

`npm install prettier@1.18.2 --save`

```js
test("测试 generateConfig 函数", () => {
  expect(generateConfig()).toMatchInlineSnapshot(
    {
      time: expect.any(Date)
    },
    `
    Object {
      "author": "kobe",
      "port": 8080,
      "server": "http://localhost",
      "time": Any<Date>,
    }
  `
  );
});
```

