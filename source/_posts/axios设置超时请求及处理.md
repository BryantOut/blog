---
title: axios设置超时请求及处理
tags: vue
categories: vue
date: 2019-2-15
---

# axios设置超时请求及处理

[原文链接](https://www.cnblogs.com/dibaosong/p/9528250.html)

> vue项目中axios请求拦截器与取消pending请求功能

在开发vue项目中，请求是不可缺少的，在发送请求时常常需要统一处理一些请求头参数等设置与响应事件，这时利用请求拦截器再好不过。

<!--more-->

这里以axios请求为例

实现了设置统一请求头添加token, 其中token在登录时被存入了localStorage中。

同时拦截器利用new cancelToken与定义的cancelPending方法实现了可以取消正在pending状态的请求，什么情况会需要取消请求呢？

如下两种情况：

1. 有一个局部分页时，用户快速点击第2页，然后继续点击第3页，如果网络不太稳定时，第2页的请求正在发送中，还未响应，但第3页的请求先响应了，过了一会第2 页请求才响应，这时用户处于第3页，但看到的数据确是第2页的，当然有人会说可以在发送请求过程中禁用掉分页按钮点击，但我感觉体验不太好，为何禁用呢，直接点击第3页时中断掉之前相同的请求即可。
2. 切换路由时，上一路由页面中仍有未响应的请求时切换了路由，应该把正在pending的所有请求中断取消掉。

# axios请求超时，设置重新请求的完美解决办法

[原文地址](https://javascript.ctolib.com/ssttm169-use-axios-well.html)

## 带坑的解决方案一

我的经验有限，觉得唯一能做的，就是axios请求超时之后做一个重新请求。通过研究 axios的使用说明，给它设置一个timeout = 6000

```js
axios.defaults.timeout =  6000;
```

然后加一个栏截器.

```js
import axios from 'axios'

// 通过研究 axios的使用说明，给它设置一个timeout
axios.defaults.timeout =  3000;

// Add a request interceptor
// Add a request interceptor
axios.interceptors.request.use(function (config) {
    // Do something before request is sent
    return config;
  }, function (error) {
    // Do something with request error
    return Promise.reject(error);
});

// Add a response interceptor
axios.interceptors.response.use(function (response) {
    // Do something with response data
    return response;
  }, function (error) {
    // Do something with response error
    return Promise.reject(error);
});

// 这个栏截器作用是 如果在请求超时之后，栏截器可以捕抓到信息，然后再进行下一步操作，也就是我想要用 重新请求。

// 北京pk10
export const gameDataForNowOpenForBjpk10 = (pa) => {
    return axios.get(`http://api.163234.com/homePage/gameDataForNowOpenForBjpk10`)
    .then(function(res) {
        return res.data
    })
    .catch(function(err) {
        //超时之后在这里捕抓错误信息.
        return err
    });
}
```

**前台处理**

```js
gameDataForNowOpenForBjpk10().then(res=>{
  // console.log(res)
  that.bjpkList = res.result;
  if (that.bjpkList.iopen==1) {
    that.isOpenNow = false
    clearInterval(that.timer2)
    clearInterval(that.timer3)
    that.opennum = that.bjpkList.sopennum.split("|");
    that.property = JSON.parse(that.bjpkList.property);
    that.countdown( that.bjpkList.nextOpenTime)
    that.kjxx=""
  }
}).catch(err=>{
  // console.log(err)
  this.getData()
})
```

**效果**

![](/mdImg/axios设置请求超时及处理.png)

**隐藏bug**

就是被请求得链接失效或其他原因造成无法正常访问的时候，这个机制就失效了它不会等待我设定的6秒，而且一直在刷，一秒种请求几十次，很容易就把服务器搞垮了，请看下图, 一眨眼的功能，它就请求了146次。