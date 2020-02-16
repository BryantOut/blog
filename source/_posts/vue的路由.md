---
title: Vue Router
tags: vue
categories: vue
date: 2018-8-31
---

# 安装

### 引入JS文件

```html
<!-- 1.引入路由文件 -->
<script src="./js/vue-router.js"></script>
```

### NPM

```no
npm install vue-router
```

# 起步

> 用 Vue.js + Vue Router 创建单页应用，是非常简单的。使用 Vue.js ，我们已经可以通过组件来组成应用程序，当你要把 Vue Router 添加进来，我们需要做的是，将组件(component)映射到路由(router)，然后告诉 Vue Router 在哪里渲染它们。

<!--more-->

# 路由初体验

### HTML

```html
<div id="app">
  <ul>
    <!-- 6.在 vue 中提供一个 router-link 的组件来实现路由的跳转，
    里面有一个 to 属性，它的值和组件规则中的 path 值必须完全一样
    router-link: 后期会被渲染为 a 标签
    to 属性后期会被渲染为锚链接
    -->
    <li><router-link to="/index">首页</router-link></li>
    <li><router-link to="/product">产品</router-link></li>
  </ul>

  <!-- 5.路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

> 要注意，当 `router-link`  对应的路由匹配成功，将自动设置 class 属性值 `.router-link-active` 

### JavaScript

```html
<script>
  var index = Vue.component('index',{
    template:`<h1>首页</h1>`
  })
  var product = Vue.component('product',{
    template:`<h1>产品</h1>`
  })
  // 2. 创建路由:使用 new VueRouter(路由的具体配置)
  var router = new VueRouter({
    routes:[
      // 【/】的作用就是指定路由的路径是相对于根目录
      {name:'index',path:'/index',component:index},
      // 再加一条路由规则
      {name:'product',path:'/product',component:product},
      // 添加路由重定向  【/】就是默认的路由
      {name:'defult',path:'/',redirect:{name:'index'}}
    ]
  })

  var vm = new Vue({
    el: '#app',
    data: {

    },
    // 4.注入路由--使用路由
    router:router
  })
</script>
```

#### routes中对象的参数

> 进行具体的路由配置。可以配置多个。所有配置都是存放在一个 routes,
>
> 里面就是一个一个对象，每个对象就对应着一条路由规则，

|    参数     | 描述        |
| :-------: | --------- |
|   name    | 路由的名称     |
|   path    | 路由的路径     |
| component | 该路由所映射的组件 |

#### 注入路由

```js
var vm = new Vue({
  el: '#app',
  data: {

  },
  // 4.注入路由--使用路由
  router:router
})
```

> 通过注入路由器，我们可以在任何组件内通过 `this.$router`  访问路由器，也可以通过 `this.$router`  访问当前路由：

#### 添加路由重定向

```js
// 添加路由重定向  【/】就是默认的路由
{name:'defult',path:'/',redirect:{name:'index'}}
```

# 动态路由匹配

![](/mdImg/Router1.png)

> 我们经常需要把某种模式匹配到的所有路由，全部映射到同一个组件。例如，我们有一个 `catagory`  组件，对于不同的分类页面，都要使用这个组件来渲染。那么，我们可以在 `vue-router` 的路由路径中使用"动态路径参数来达到这个效果"

```html
<li>
  <router-link to='/user/13433223834'>13433223834用户</router-link>
</li>
<li>
  <router-link to='/user/13490055271'>13690055271用户</router-link>
</li>
```

## 定义路由规则

```js
var router = new VueRouter({
  routes: [
    {
      name: 'user',
      path: '/user/:userName',
      component: user
    }
  ]
})
```

## 根据路由规则定制组件

```js
var user = Vue.component('user',{
  template: '<div><h1>用户:{{userName}}</h1><img src="./images/timg (9).jpg" alt=""></div>',
  data() {
    return {
      userName: '',
    }
  },
  mounted() {
    console.log(this.$route.params)
    this.userName = this.$route.params.userName;
  },
  // 响应路由参数的变化!!!!!!
})
```

> 现在呢，像 `catagory/1` 和 `catagory/2` 都将映射到相同的路由。

### $route

> 一个“路由参数”使用冒号 `:` 标记。当匹配到一个路由时，参数值会被设置到 `this.$route.params`，可以在每个组件内使用。于是，我们可以更新 `catagory` 的模板，输出当前分类的id

```js
this.userName = this.$route.params.userName
```

你可以在一个路由中设置多段"路径参数"，对应的值都会设置到 `$route.params` 中。例如

|              模式               |        匹配路径         | $route.params                      |
| :---------------------------: | :-----------------: | ---------------------------------- |
|        /user/:username        |     /user/evan      | { username: 'evan' }               |
| /user/:username/post/:post_id | /user/evan/post/123 | { username: 'evan', post_id: 123 } |

除了 `$route.params` 外，`$route` 对象还提供了其它有用的信息，例如，`$route.query`

## 响应路由参数的变化

提醒一下，当使用路由参数时，例如从 `/catagory/catton`  导航到 `/catagory/movie` ，**原来的组件实例会被复用。**因为两个路由都渲染同一个组件，比起销毁再创建，复用则则显得更加高效。**不过，这也意味着组件的生命周期钩子不会再被调用。**

> 复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch （监测变化）`$route `对象

```js
mounted() {
  ……
},
  watch:{
   '$route'(to,from) {
     // ……对路由变化做出响应
     this.userName = this.$route.params.userName
   }
 }
```

或者使用 `beforeRouteUpdate` 导航守卫

## 匹配优先级

有时候，同一个路径可以匹配多个路由，此时，匹配地优先级就按照路由地定义顺序：谁先定义的，谁的优先级就是高。

# 嵌套路由

实际生活中的应用界面，通常由多层嵌套的组件组合而成。

同样地，URL 中各段路径也按某种结构对应嵌套的各层组件，例如：

```html
/catagory/carton                      /catagory/movie
+------------------+                  +-----------------+
| catagory         |                  | catagory        |
| +--------------+ |                  | +-------------+ |
| | carton       | |  +------------>  | | movie       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

借助 `vue-router` ，使用嵌套路由配置，就可以简单地表达这种关系。

```html
<div id="app">
  <ul>
    <li>
      <router-link to='/category/catton'>卡通</router-link>
    </li>
    <li>
      <router-link to='/category/movie'>电影</router-link>
    </li>
  </ul>
  <router-view></router-view>
</div>
```

> 这里地 `<router-view></router-view>` 是最顶层地出口，渲染最高级路由匹配到地组件。同样地，一个被渲染组件同样可以包含自己地嵌套 `<router-view></router-view>` 。例如，在 `catagory`  组件地模板添加一个 `<router-view></router-view>` 

```js
var catagory = Vue.component('catagory', {
  // 通过 $route.params 来获取参数，它是一个对象，里面的成员就是在路由规则中定义的参数
  template: `<div>
                <h1>分类之</h1>
                <h1>{{className}}</h1>
                <button @click='detail'>点击查看详细信息</button>
                <router-view></router-view>
             </div>`,
}
```

> 要在嵌套的出口中渲染组件，需要在 `VueRouter`  的参数中使用 `children`  配置

```js
var router = new VueRouter({
  routes: [{
    name: 'index',
    path: '/index',
    component: index
  },
           {
             name: 'category',
             path: '/category/:className',
             component: category,
             children: [
               {
                 name: 'detailCatton',
                 path: 'detailCatton',
                 component: detailCatton
               },
               {
                 name: 'detailMovie',
                 path: 'detailMovie',
                 component: detailMovie
               }
                       ]
           }
          ]
})
```

**要注意，以 `/` 开头的嵌套路径会被当作根路径。这让你充分的使用嵌套组件而无须设置嵌套的路径。**

你会发现， `children`  配置就像是 `routes`  配置一样的路由配置数组，所以呢，你可以嵌套多层路由。

如果你想渲染点什么，可以提供一个子路由

## 编程式导航

1. 除了使用 `<router-link>` 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。

`this.$router.push(location,onComplete?,onAbort?)`

**注意：在 Vue 实例内部，你可以通过 `$router`  访问路由实例。因此你可以调用 `this,$router.push`**

2. 想要导航到不同的 URL ，则使用 `route.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则会回到之前的 URL。
3. 当你点击 `<router-link>`时，这个方阿飞会在内部调用，所以说，点击 `<router-link :to="...">` 等同于调用 `router.push(...)`。

|            声明式            |       编程式        |
| :-----------------------: | :--------------: |
| `<router-link :to="...">` | router.push(...) |

```html
methods: {
  detail() {
    if (this.className == 'catton') 
	  {
      	this.$router.push({name:'detailCatton'})
      } 
	  else if (this.className == 'movie')
	  {
      	this.$router.push({name:'detailMovie'})
    }
  }
}
```

**注意：如果提供了 `psth` ,`params` 会被忽略，上述例子中的 `query` 并不属于这种情况。**

![](/mdImg/Router2.png)

## 视图命名

>  一个组件内部有多个router-view怎么来分配组件呢？比如三栏布局，顶部栏点击按钮，左侧的菜单变化

**router.js**

```js
{
    path: '/about',
    name: 'about',
    component: () => import(/* webpackChunkName: "about" */ './views/About.vue'),
    children: [
      {
        path: 'detail',
        components: {
          default: com1,
          special: com2
        }
      }
    ]
  }
```

**about.vue**

```vue
<template>
  <div class="about">
    <h1>This is an about page</h1>
    <router-view></router-view>
    <router-view name="special"></router-view>
  </div>
</template>
```

# 导航守卫

[官方文档](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%85%A8%E5%B1%80%E8%A7%A3%E6%9E%90%E5%AE%88%E5%8D%AB)

> **译者注；**
>
> "导航"表示路由正在发生改变。

正如其名，`vue-router` 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：

全局的，单个路由独享的，或者组件级的。

记住**参数或查询的改变并不会触发进入/离开的导航守卫**。你可以通过观察 `$route` 对象来应对这些变化，或使用 ``beforeRouteUpdate` ` 的组件内守卫。

## 全局守卫

你可以使用 `route.beforeEach` 注册一个全局前置守卫：

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**。

每个守卫方法接收三个参数：

| 参数            | 描述                                       |
| ------------- | ---------------------------------------- |
| to:Route      | 即将要进入的目标--路由对象                           |
| from:Route    | 当前导航正要离开的路由                              |
| next:Function | 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。 |

![](/mdImg/resolve.png)

**确保要调用 `next` 方法，否则钩子就不会被 resolved。**

> 需求：**如果没有登录，但你访问其他需要登录的页面，那我就让你跳到登录页面去**

**使用全局守卫实现需求代码**

> **每次路由跳转都会触发**

```js
// 你可以使用 router.beforeEach 注册一个全局前置守卫：
router.beforeEach((to, from, next) => {
  console.log(to.path)
  var token = localStorage.getItem('mytoken')
  // 如果没有 token ，判断如果是 to 到 /login,next
  if (token) {
    next()
  } else {
    if (to.path === '/login') {
      next()
    } else {
      next({
        path: '/login'
      })
    }
  }
})

router.beforeResolve((to, from, next) => {
    // 全局解析守卫，2.5.0新增，这和 router.beforeEach 类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件解析之后，解析守卫就被调用
    next()
})

router.afterEach((to,from) => {
    // 全局后置钩子
})
```

## 路由独享的守卫

> 写在配置里，你可以在路由配置上直接定义 `beforeEnter `守卫

```js
const router = new VueRouter({
    routes: [
        {
            path: `/foo`,
            compontent: Foo,
            // 在进入这个路由之前调用
            beforeEnter: (to, from, next)=>{
                // ...
            }
        }
    ]
})
```



## 组件内的守卫

> 最后，你可以在路由组件内直接定义以下路由导航守卫

```js
beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没有被创建
},
beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带动态参数的路径 /foo/:id,在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
},
 beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
    // 通常用来禁止用户还未保存修改前突然离开。该导航可以通过 next(false) 来取消 
}
```

