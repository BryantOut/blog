---
title: vue源码解析
tags: vue
categories: vue
date: 2019-10-8
---

# Vue工作机制

[数据劫持](https://www.jianshu.com/p/87a1ff1d7a3c)

>  **vue** 的核心是减少页面渲染的次数和数量

## 初始化

在 `new Vue()` 之后。Vue会调用进行初始化，会初始化生命周期、事件、props、methods、data、computed与watch等。其中最重要的是通过 `Object.defineProperty` 设置 `setter` 与 `getter` ，用来实现 【**响应式**】以及 【**依赖收集**】。

初始化之后调用 `$mount` 挂载组件

![](/mdImg/vue1.png)

![](/mdImg/vue2.png)

### 编译

编译模块分为三个阶段

1. parse--解析

> 使用正则解析 template 中的 vue 指令（v-xxx）变量等等，形成语法树 AST

2. optimize [ˈɒptɪmaɪz]--优化

> 标记一些静态节点，用作后面的性能优化，在diff的时候略过

3. generate--生成

> 把第一步生成的 AST 转化为渲染函数 render function

### 响应式

这一块是vue最核心的内容

getter 和 setter 待会咱们会演示代码，初始化的时候通过 defineProperty 进行绑定，设置通知的机制

当编译生成的渲染函数被实际渲染的时候，会触发 getter 进行以来依赖收集，在数据变化的时候，触发 setter 进行更新

### 虚拟dom

Virtual Dom 是 react 首创，Vue2开始支持，就是用 JavaScript 对象来描述dom结构，数据修改的时候，我们先修改虚拟dom中的数据，然后数组做diff，最后再汇总所有diff，力求做最少的dom操作，毕竟js里对比很快，而真实的dom操作太慢

![](/mdIMg/vue5.png)

### 更新视图

数据修改触发setter，然后监听器会通知进行修改，通过对比两个dom树，得到改变的地方，就是patch然后只需要把这些差异修改即可

### Vue响应式的原理deﬁneProperty 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <p id="name"></p>
    </div>

    <script>
        var obj = {};
        Object.defineProperty(obj,'name',{
            get :function () {
                return document.querySelector('#name').innerHTML;
            },
            set :function (val) {
                document.querySelector('#name').innerHTML = val
            }
        })
        obj.name = 'Curry'
    </script>
</body>
</html>
```

![](/mdImg/vue3.png)

## 实现数据响应式

### 依赖收集与追踪

```js
new Vue({
    template:
    	`<div>
			<span>{{name1}}</span>
            <span>{{name2}}</span>
            <span>{{name1}}</span>
        </div>`,
    data: {
        name1: 'name1',
        name2: 'name2',
        name3: 'name3'
    },
    created() {
        this.name1 = 'curry'
        this.name3 = 'Jerry'
    }
})
```

name1被修改，视图更新，需要更新两处

name3没用到，不需要更新

如何实现呢，需要扫描视图收集依赖，知道视图中到底哪些地方对数据有依赖，这样当数据变化时就能有的放矢了。

### 编译compile

```
核心逻辑，获取 dom ，遍历 dom ，获取{{}}格式的变量，以及每个 dom 的属性，截获k-和@开头的设置响应式
```

![](/mdImg/vue4.png)

### 结束

