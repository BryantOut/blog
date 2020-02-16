---
title: vue中的Js动画与Velocity.js的结合
tags: vue
categories: vue
date: 2019-3-20
---

# 在过渡钩子函数中使用 JavaScript 直接操作 DOM

```html
<div id="app">
  <button @click='isShow = !isShow'>切换显示和隐藏</button>
  <transition name='diyslide' 
              v-on:before-enter="beforeEnter"
              v-on:enter="enter"
              v-on:after-enter="afterEnter"
              v-on:enter-cancelled="enterCancelled"
              v-on:before-leave="beforeLeave"
              v-on:leave="leave"
              v-on:after-leave="afterLeave"
              v-on:leave-cancelled="leaveCancelled">
    <img src="./images/timgdfdf.jpg" alt="" v-show='isShow' >
  </transition>
</div>
```

<!--more -->

```html
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      isShow: false
    },
    methods: {
      // --------
      // 进入中
      // --------

      beforeEnter: function (el) {
        // el代表transition包裹的标签
        console.log('beforeEnter')
        el.style.marginLeft = '200px'
      },
      // 此回调函数是可选项的设置
      // 与 CSS 结合时使用
      enter: function (el, done) { 
        console.log('enter')
        // 从事件中不能直接添加过渡效果
        var left = 200;
        var timerId = setInterval(()=>{
          left--;
          el.style.marginLeft = left + 'px';
          if(left == 0){
            clearInterval(timerId)
          }
        },17)
        // 如果没有调用 done 函数，那么 enter 会阻塞后面的 afterenter 的执行，
        // 同时认为过渡动画已经结束，结束 afterenter 事件不执行
        done()
      },
      afterEnter: function (el) {
        el.style.marginLeft = '0px'
        console.log('afterEnter')
      },

      // --------
      // 离开时
      // --------

      beforeLeave: function (el) {
        // ...
      },
      // 此回调函数是可选项的设置
      // 与 CSS 结合时使用
      leave: function (el, done) {
        // ...
        done()
      },
      afterLeave: function (el) {
        // ...
      },
      // leaveCancelled 只用于 v-show 中
      leaveCancelled: function (el) {
        // ...
      }
    }
  })
</script>
```

# 

# vue中的Js动画与Velocity.js的结合

[官方网址](http://velocityjs.org/)

![](/mdImg/velocity1.png)

![](/mdImg/velocity2.png)

![](/mdImg/velocity3.png)

![](/mdImg/velocity4.png)

![](/mdImg/velocity5.png)

![](/mdImg/velocity6.png)