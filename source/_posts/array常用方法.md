---
title: 163开发期间遇到的问题
tags: vue项目
categories: vue项目
date: 2019-3-12
---

# vue兼容安卓低版本

[文章地址](https://blog.csdn.net/yy110621/article/details/82889434)

<!--more-->

# 完美解决IE低版本不支持call与apply的问题

[文章地址](https://blog.csdn.net/u011714480/article/details/17143563)

> Function.prototype的apply和call是在1999年发布的ECMA262 Edition3中才加入的（1998年发布ECMA262 Edition2）。在此前的的浏览器如IE5.01(JScript 5.0)中是没有apply和call的。因此会带来一些兼容性问题，以下是修复方式：

```js
if(!Function.prototype.apply){
    Function.prototype.apply = function(obj, args){
        obj = obj == undefined ? window : Object(obj);//obj可以是js基本类型
        var i = 0, ary = [], str;
        if(args){
            for( len=args.length; i<len; i++ ){
                ary[i] = "args[" + i + "]";
            }
        }
        obj._apply = this;
        str = 'obj._apply(' + ary.join(',') + ')';
        try{
            return eval(str);
        }catch(e){
        }finally{
            delete obj._apply;
        }   
    };
}
if(!Function.prototype.call){
    Function.prototype.call = function(obj){
        var i = 1, args = [];
        for( len=arguments.length; i<len; i++ ){
            args[i-1] = arguments[i];
        }
        return this.apply(obj, args);
    };
}
```

# visibilitychange事件判断当前页面——可见性的状态

[文章地址](https://www.cnblogs.com/yc-code/p/4553934.html)

> 页面上组件有用到定时器的情况下，离开页面的时候是停止的，回到页面并不是即使同步的效果，监听到重新回到页面可以重新加载组件

```js
// 各种浏览器兼容
var hidden, state, visibilityChange;
if (typeof document.hidden !== "undefined") {
  hidden = "hidden";
  visibilityChange = "visibilitychange";
  state = "visibilityState";
} else if (typeof document.mozHidden !== "undefined") {
  hidden = "mozHidden";
  visibilityChange = "mozvisibilitychange";
  state = "mozVisibilityState";
} else if (typeof document.msHidden !== "undefined") {
  hidden = "msHidden";
  visibilityChange = "msvisibilitychange";
  state = "msVisibilityState";
} else if (typeof document.webkitHidden !== "undefined") {
  hidden = "webkitHidden";
  visibilityChange = "webkitvisibilitychange";
  state = "webkitVisibilityState";
}

var that = this
// 添加监听器，在title里显示状态变化
document.addEventListener(visibilityChange, function() {
  // document.title = document[state];
  if (!document.hidden) {
    // console.log('刷新页面')
    // location.reload()
    that.hackReset = false
    that.$nextTick(() => {
      that.hackReset = true
    })
  }
}, false);
```

# vue强制刷新组件

[文章地址](https://blog.csdn.net/guapizz/article/details/79214267)

```js
<template>
  <div v-if='show9&&hackReset'>
    <beipk10></beipk10>
  </div>
</template>
<script>
  that.hackReset = false
  that.$nextTick(() => {
    that.hackReset = true
  })
</script>
```

# H5移动端端按钮添加按下效果

```js
// 按钮touch事件
document.getElementsByClassName('btn')[0].ontouchstart = function() {
  document.getElementsByClassName('btn')[0].classList.add('active')
}
document.getElementsByClassName('btn')[0].ontouchend = function() {
  document.getElementsByClassName('btn')[0].classList.remove('active')
}
```

# vue中使用vue-router切换页面时滚动条自动滚动到顶部的方法

[文章地址](https://blog.csdn.net/csl125/article/details/83996314)

# js 判断当前操作系统是ios还是android还是电脑端

[文章地址](https://www.cnblogs.com/zhuchenglin/p/7528250.html)