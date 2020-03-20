---
title: 前端测试——Jest的匹配器（matchers）
tags: Jest
categories: Jest
date: 2020-03-17
---

# 真假有关的匹配器

## `toBe`匹配器

```js
// matchers.test.js
test('测试10与10相匹配', () => {
    // toBe 匹配器 matchers 相当于 === 引用地址必须也相同
    const a = { one: 1 }
    expect(a).toBe({ one: 1 })
});
```

<!--more-->

**配置，让 `jest` 监听所有测试文件的变化**

```js
// package.json
{
    "name": "day02",
    "version": "1.0.0",
    "description": "",
    "main": "math.js",
    "scripts": {
        "test": "jest --watchAll" // 新增--watchAll
    },
    "author": "",
    "license": "ISC",
    "devDependencies": {
        "@babel/core": "^7.4.5",
        "@babel/preset-env": "^7.4.5",
        "jest": "^24.8.0"
    }
}
```

**运行**

![](/mdImg/test6.png)

## `toEqual`匹配器

> 只匹配内容

```js
test('测试10与10相匹配', () => {
    const a = { one: 1 }
    expect(a).toEqual({ one: 1 })
});
```

![](/mdImg/test7.png)

## `toBeNull` 匹配器

```js
test('是否为null', () => {
    const a = null
    expect(a).toBeNull()
});
```

![](/mdImg/test8.png)

## `toBeUndefined` 匹配器

```js
test('是否为Undefined', () => {
    const a = undefined;
    expect(a).toBeUndefined();
});
```

![](/mdImg/test9.png)

## `toBeDefined` 匹配器

```js
test('是否为defined', () => {
    const a = '科比';
    expect(a).toBeDefined();
});
```

![](/mdImg/test10.png)

## `toBeTruthy` 匹配器

```js
test('是否为true', () => {
    const a = null;
    expect(a).toBeTruthy();
});
```

![](/mdImg/test11.png)

## `toBeFalsy 匹配器

```js
test('是否为false', () => {
    const a = null;
    expect(a).toBeFalsy();
});
```

![](/mdImg/test12.png)

## `not` 匹配器

```js
test('not匹配器', () => {
    const a = null;
    expect(a).not.toBeTruthy();
});
```

![](/mdImg/test13.png)

# 数字相关的匹配器

## `toBeGreaterThan`匹配器

```js
test('toBeGreaterThan大于', () => {
    expect(10).toBeGreaterThan(11);
});
```

![](/mdImg/test14.png)

## `toBeLessThan`匹配器

```
test('toBeGreaterThan小于', () => {
    expect(10).toBeLessThan(11);
});
```

## `toBeLessThanOrEqual`匹配器

```js
test('toBeLessThanOrEqual小于或等于', () => {
    expect(10).toBeLessThanOrEqual(10);
});
```

## `toBeCloseTo`  匹配器

```js
test('toEqual', () => {
    const firstNum = 0.1;
    const secondNum = 0.2;
    expect(firstNum + secondNum).toEqual(0.3);
});
```

![](/mdImg/test15.png)

```js
test('toBeCloseTo', () => {
    const firstNum = 0.1;
    const secondNum = 0.2;
    expect(firstNum + secondNum).toBeCloseTo(0.3);
});
```

![](/mdImg/test16.png)

# string相关的选匹配器

## `toMatch`匹配器

```js
test('toMatch字符串中是否包含', () => {
    const str = 'kobe.com';
    expect(str).toMatch('kobe');
});
```

# Array相关的匹配器

```js
test('toContain数组中是否包含', () => {
    const arr = ['kobe', 'curry', 'kobe'];
    const newArr = new Set(arr); // 数组去重
    expect(newArr).toContain('curry');
});
```

# 异常匹配器

## 是否抛出异常

```js
const throwNewErrorFunc = () => {
    throw new Error('this is a new error');
}

test('toThrow', () => {
    expect(throwNewErrorFunc).not.toThrow();
    // 函数运行结果抛出异常
})
```

![](/mdImg/test17.png)

## 抛出异常，且内容是否一致

```js
const throwNewErrorFunc = () => {
    throw new Error('this is a new error');
}

test('toThrow', () => {
    expect(throwNewErrorFunc).toThrow('this is a new error');
})
```

