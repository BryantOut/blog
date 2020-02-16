---
title: vue过渡&动画
tags: vue
categories: vue
date: 2019-3-20
---

# 隐藏到显示【图解】

![](/mdImg/transition动画.png)

<!--more-->

# 显示到隐藏【图解】

![](/mdImg/transition动画2.png)

>  示例代码

![](/mdImg/transition动画3.png)

>  简化代码

![](/mdImg/transition动画4.png)

>  Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入或离开的过渡

| 情形   |             |
| ---- | ----------- |
| 条件渲染 | (使用 v-if)   |
| 条件展示 | (使用 v-show) |
| 动态组件 |             |

# transition的作用

> **将需要添加过渡的元素用transition组件包裹起来，Vue 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果。**

# 在 CSS 过渡和动画中自动应用 class

## 在进入/离开的过渡中，会有6个 class 切换

|       类名       | 描述                                       |
| :------------: | :--------------------------------------- |
|    v-enter     | 定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。   |
| v-enter-active | 定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，<br />在元素被插入之前生效，在过渡/动画完成之后移除。<br />这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。 |
|   v-enter-to   | **2.1.8版及以上** 定义进入过渡的结束状态。在元素被插入之后下一帧生效<br /> (与此同时 `v-enter` 被移除)，在过渡/动画完成之后移除。 |
|    v-leave     | 定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。        |
| v-leave-active | 定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，<br />在离开过渡被触发时立刻生效，<br />在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，<br />延迟和曲线函数。 |
|   v-leave-to   | **2.1.8版及以上** 定义离开过渡的结束状态。<br />在离开过渡被触发之后下一帧生效 (与此同时 `v-leave` 被删除)，<br />在过渡/动画完成之后移除。 |

![](/mdImg/transition1.png) 

```html
<div id="app">
  <button @click='isShow = !isshow'>切换显示和隐藏</button>
  <!-- 如果使用过渡样式添加过渡效果
  1.将你想添加过渡效果的元素使用 transition 包含
  2.必须为这个 transition 设置名称，这个名称就是你后期的过渡样式的前缀
  	,意味着后期的样式就是以这个名称作为前缀的
  -->
  <transition name='diyslide'>
    <p v-show='isshow'>我是被控制的元素</p>
  </transition>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      isShow : false
    }
  })
</script>
```

```css
<style>
  /* 添加过渡样式，这个样式的前缀就是之前transition标签中定义的name */
  .diyslide-enter {
    margin-left: 300px;
    opacity: 0;
  }
  .diyslide-enter-active {
    transition: margin-left 2s,opacity 2s;
  }
  .diyslide-enter-to {
    margin-left: 0;
    opacity: 1;
  }
  .diyslide-leave {
    opacity: 1;
    margin-left: 0;
  }
  .diyslide-leave-active {
    transition: margin-left 2s,opacity 2s;
  }
  .diyslide-leave-to {
    opacity: 0;
    margin-left: 300px;
  }
</style>
```

![](/mdImg/transition2.png)

# 可以配合使用第三方 CSS 动画库，如 Animate.css

```html
<link rel="stylesheet" href="./css/animate.css">
```

## 我们可以通过以下特性来自定义过渡类名

| 类名                      |      |
| ----------------------- | ---- |
| enter-class             |      |
| enter-active-class      |      |
| enter-to-class (2.1.8+) |      |
| leave-class             |      |
| leave-active-class      |      |
| leave-to-class (2.1.8+) |      |

> 他们的优先级高于普通的类名，这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如 Animate.css ,结合十分有用

```html
<div id="app">
  <button @click='isShow = !isShow'>切换显示和隐藏</button>
  <transition 
	enter-active-class="animated tada" 
	leave-active-class="animated zoomOut">
    <p v-show = 'isShow'>广东省潮州市湘桥区</p>
  </transition>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      isShow: false
    }
  })
</script>
```

![](/mdImg/transition3.png)

[Animate.css](https://daneden.github.io/animate.css/)

# vue结合动画函数

![](/mdImg/transition动画5.png)

# 让页面元素第一次显示的时候便执行一次动画

![](/mdImg/transition动画6.png)

# 在Vue中同时是用过渡和动画

![](/mdImg/transition动画7.png)

> 除了库里面@keyframe的动画，自己再写transition过渡动画

![](/mdImg/transition动画7.png)

> 手动设置一哪种动画的动画时常为标准

# 自定义动画时长

![](/mdImg/transition动画10.png)

## 入场动画和离开动画时长分别设置

![](/mdImg/transition动画11.png)

