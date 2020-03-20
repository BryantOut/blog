---
title: 前端测试4_Jest异步代码的测试方法
tags: Jest
categories: Jest
date: 2020-03-19
---

# 异步测试方法一

```js
// fetchData.js
import axios from 'axios';

export const fetchData = (fn) => {
    axios.get('http://www.dell-lee.com/react/api/demo.json').then((res) => {
        fn(res.data)
    })
}
```

<!--more-->

```js
// fetchData.test.js
import { fetchData } from './fetchData';
test('fetchData 返回结果为 { success: true }', () => {
    fetchData((data) => {
        expect((data).toEqual({ success: true }))
    })
})
```

> 这样写有问题，因为这种写法不回去等异步结果，而函数直接运行结束，无论接口对不对，返回结果是不是成功。测试结果皆为成功。

```js
// fetchData.test.js
import { fetchData } from './fetchData';
test('fetchData 返回结果为 { success: true }', (done) => {
    fetchData((data) => {
        expect(data).toEqual({ success: true })
        done();
    })
})
```

> 新增done参数，异步执行完执行，测试用例才结束

# 异步测试方法二

> fetchData返回的是一个promise所以记得加上return

```js
// fetchData.js
import axios from 'axios';

export const fetchData = (fn) => {
    return axios.get('http://www.dell-lee.com/react/api/demo.json')
}

// fetchData.test.js
import { fetchData } from './fetchData';
test('fetchData 返回结果为 404', () => {
    return fetchData().then((res) => {
        expect(res.data).toEqual({ success: true })
    })
})
```

### 是否包含404

```js
// fetchData.test.js
import { fetchData } from './fetchData';
test('fetchData 返回结果为 404', () => {
    return fetchData().catch((e) => {
        expect(e.toString().indexOf('404') > -1).toEqual(true)
    })
})
```

> expect.assertions(1) 至少执行一次

```js
// fetchData.test.js
import { fetchData } from './fetchData';
test('fetchData 返回结果为 404', () => {
    expect.assertions(1);
    return fetchData().catch((e) => {
        expect(e.toString().indexOf('404') > -1).toEqual(true)
    })
})
```

![](/mdImg/test25.png)

# 异步测试方法三

**成功的情况**

```js
// fetchData.test.js
import { fetchData } from './fetchData';
test('fetchData 返回结果为 404', () => {
    return fetchData().catch((e) => {
        expect(e.toString().indexOf('404') > -1).toEqual(true)
    })
})

// fetchData.test.js
import { fetchData } from './fetchData';
test('fetchData 返回结果为 抛出异常', () => {
    return expect(fetchData()).resolves.toMatchObject({ data: { success: true } })
})
```

**失败的情况**

```js
// fetchData.test.js
import { fetchData } from './fetchData';

test('fetchData 返回结果为 抛出异常', () => {
    return expect(fetchData()).rejects.toThrow();
})
```

![](/mdImg/test26.png)

# 异步测试方法四

```js
test('fetchData 返回结果为 抛出异常', async() => {
    await expect(fetchData()).rejects.toThrow();
})
```

# 异步测试方法五

```js
// 成功的情况
test('fetchData 返回结果为 成功', async() => {
    const res = await fetchData();
    expect(res.data).toEqual({ success: true });
})

// 失败的情况
test('fetchData 返回结果为 抛出异常', async() => {
    try {
        await fetchData();
    } catch (e) {
        expect(e.toString()).toEqual('Error: Request failed with status code 404')
    }
})
```

