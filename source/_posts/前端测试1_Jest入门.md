---
title: 前端测试——Jest入门
tags: Jest
categories: Jest
date: 2020-03-16
---

## 针对小模块math.js,引入jest进行单元化测试

### 小模块 math.js

```js
function add(a, b) {
    return a + b;
}

function minus(a, b) {
    return a - b + 100;
}
try {
    module.exports = {
        add,
        minus
    }
} catch (error) {

}
```

<!--more-->

### 针对该模块写测试代码

```js
const math = require('./math.js');
const { add, minus } = math;

test('测试加法 3+3', () => {
    expect(add(3, 3)).toBe(6)
});
test('测试减法 3-3', () => {
    expect(minus(3, 3)).toBe(0)
});
```

### 安装

`npm i jest@24.8.0 -D`

> `-D` 是因为只用于开发环境

### 修改配置文件 `package.json` 内容

```js
"scripts": {
    "test": "jest"
},
```

### 测试

`npm run test`

> 自动识别.test.js结尾的文件

![](/mdImg/test1.png)

## 对jest一些默认配置进行修改

### 将jest的一些配置项暴露出来，让他能够配置

`npx jest --init `

![](/mdImg/test2.png)

![](/mdImg/test3.png)

### 生成测试覆盖率报告

`npx jest --coverage`

![](/mdImg/test4.png)

## 结合babel

使用 `babel` 这个工具将 `EsModul` 的代码转换成 `commonJS` 的代码 ，`jest` 就可以测试了

> 一般我们不会用 `commonJS` 的模块导出或者引入的方式，对模块进行定义或者使用，一般我们会使用 `esModule` 的形式

### 安装babel

`npm install @babel/core@7.4.5 @babel/preset-env@7.4.5 -D`

> 使用babel我们一般会在项目根下新建 `.babelrc` 进行一些配置

```js
{
    "presets": [
        [
            "@babel/preset-env",{
                "targets": {
                    "node": "current"
                }
            }
        ]
    ]
}
```

![](/mdImg/test5.png)

```js
// math.js
export function add(a, b) {
    return a + b;
}

export function minus(a, b) {
    return a - b + 100;
}

// math.test.js
import { add, minus } from './math.js'
```

