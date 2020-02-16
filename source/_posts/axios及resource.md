---
title: axios
tags: vue
categories: vue
date: 2018-8-29
---

> Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

[axios文档](https://github.com/axios/axios)

[简书相关文档](https://www.kancloud.cn/yunye/axios/234845)

![](/mdImg/请求报文.png)

```html
 <script src="./js/axios.js"></script>
```

# # get 请求示例

```html
<div id="app">
  <button @click='getdata'>发起get请求</button>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {

    },
    methods: {
      getdata() {
        axios.get('http://www.liulongbin.top:3005/api/getprodlist')
          .then((result) => {
          // 响应成功之后的回调
          console.log(result)
        })
          .catch((err) => {
          // 响应失败之后的回调
          console.log(err)
        })
      }
    }
  })
</script>
```

------

<!--more-->

# # post 请求示例

```html
<div id="app">
  <input type="text" v-model='newbrand'>
  <button @click='getdata'>发起post请求</button>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      newbrand:''
    },
    methods:{
      getdata(){
        // 发送 axios 请求
        axios.post('http://www.liulongbin.top:3005/api/addproduct',{
          name:this.newbrand
        })
          .then((result) => {
          // 响应成功之后的回调
          console.log(result)
        })
          .catch((err) => {
          // 响应失败之后的回调
          console.log(err)
        })
          .then(() => {
          console.log('这块代码无论如何都会执行')
        })
      }
    }
  })
</script>
```

## 响应处理交给用户

**index.js…\api**

```js
import axios from 'axios';
// 配置基准路径
const baseURL = 'http://127.0.0.1:8888/api/private/v1/'
// 设置默认的基准路径
axios.defaults.baseURL = baseURL

// 向登录接口发生请求
export const login = (params) => {
  return axios.post('login', params)
    .then((result) => {
        return result.data
        // 直接将数据返回，不在这边做处理
    })
}
```

**login.vue…\views**

```js
login(this.loginFrom).then(res => {
  console.log(res);
  if (res.meta.status === 200) {
    // 给出提示信息
    this.$message({
    message: res.meta.msg,
    type: "success"
})

/* 除了使用 <router-link> 创建 a 标签来定义导航链接，
  我们可以借助 router 的实例方法，通过编写代码来实现 */
// 路由跳转
	this.$router.push({name: 'Home'})
} else {
    this.$message({
    message: res.meta.msg,
    type: "error"
  	})
  }
});
```

![](/mdImg/axios2.png)

# # 两种请求在传递数据为对象时的区别

![](/mdImg/axios1.png)



------

# # resource

```html
<script src="./js/axios.js"></script>
<!-- 先引入vue2再引入vue-resource.因为vue-resource是基于vue2的，
引入vue-resource之后，会在vue实例上挂载一个$http对象 -->
<script src="./js/vue-resource.js"></script>
```

## ## vue-resource jsonp请求

```js
methods: {
    jsonp() {
      this.$http.jsonp('http://157.122.54.189:9093/jsonp', {
        name: 'jack'
      }, (result) => {
        console.log(result)
      }, (err) => {
        console.log(err)
      })
    }
}
```

##  ## vue-resource get 请求

```js
methods: {
  get() {
    this.$http.get('http://www.liulongbin.top:3005/api/getprodlist', (result) => {
      console.log(result)
    }, (err) => {
      console.log(err)
    })
  }
}
```

# # axios拦截器--Interceptors

![](/mdImg/axios拦截器.png)

> **You can intercept requests or responses before they are handled by `then` or `catch`.**
>
> 它们被处理之前，您可以拦截请求或响应`then`或`catch`。

**拦截器基本结构**

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });
```

> 需求：**除了登录接口，其他所有接口请求头必须设置为Authorization=token (token为登录成功后服务器返回的认证token)**

**实现需求代码**

```js
// 添加拦截器--不是我们来调用的，而是 axios 自动调用
// Add a request interceptor
axios.interceptors.request.use(function (config) {
  var token = localStorage.getItem('mytoken')
  if (token) {
    // 2.将值传递到服务器 config--
    config.headers['Authorization'] = token
  }
  return config;
}, function (error) {
  // Do something with request error
  return Promise.reject(error);
});
```

