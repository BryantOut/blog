---
title: Vue keep-alive
tags: vue
categories: vue
date: 2018-12-10
---

# vue单页 使用keep-alive页面返回不刷新

> <keep-alive>是Vue的内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染DOM。

1. 首先在App.vue页面上有下面一段代码，我们都知道这是页面渲染的地方

```js
<router-view></router-view>
```

2. 把这段代码改成如下：

```js
<keep-alive>
    <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
<router-view v-if="!$route.meta.keepAlive"></router-view>
```

**我们能看到这段代码做的逻辑判断，当路由的meta属性的keepAlive属性值为true时页面的状态保存，其他情况下不保存状态。**

3. 然后就是给我们路由设置keepAlive属性值，比如我是给主页（列表页）的路由设置了keepAlive属性为true。

```js
{
  path: '/Game',
    name: 'Game',
      component: Game,
        meta: {
          title: '博讯娱乐',
            keepAlive: true // 需要被缓存
        }
}
```

