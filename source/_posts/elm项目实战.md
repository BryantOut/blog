---
title: elm项目实战
tags: vue项目
categories: vue项目
date: 2018-9-12
---

# Vue.js核心思想

## 数据驱动

**（MVVM框架示意图）**

1. Model对应javascript对象
2. View对应dom对象
3. ViewModel就是一个vue实例

![](/mdImg/elm项目实战2.png)

<!--more-->

> 比如以前我们通过ajax从后端获取数据，为了让视图改变，我们会手动触发 dom的改变；再比如我们通过前端交互改变一些数据，为了让视图也发生一些变化，仍然需要手动去触发一些dom的变化，但手动改编dom不仅是一项繁琐而且还非常容易出错

![](/mdImg/elm项目实战1.png)

> 而我们用了vue.js后，就省去了操作dom的步骤了，vue.js里你只需要改变数据，vue.js中通过directives去dom进行一层封装。会通过指令去修改对应的dom，数据驱动dom的变化，dom是数据的一种动态映射

![](/mdImg/elm项目实战3.png)

> vue.js还会对操作进行监听，修改视图的时候，vue.js会监听到这些变化，从而改变数据，行成数据的双向绑定。

## 数据相应原理

![](/mdImg/elm项目实战4.png)

## 组件化

![](/mdImg/elm项目实战5.png)

## 组件设计原理

1. 页面上每个独立的可视/可交互区域视为一个组件
2. 每个组件对应一个工程目录，组件所需要的各种资源在这个目录下就近维护
3. 页面不过是组件的容器，组件可以嵌套自由组合形成完整的页面

# 引用单文件组件

[完整单文件组件笔记](http://localhost:4000/2018/09/03/vue%E5%8D%95%E6%96%87%E4%BB%B6%E7%BB%84%E4%BB%B6/)

## 定义单文件组件(Header.vue)

```vue
<template>
    <div class="header">
        头部
    </div>
</template>

<script>
export default {

}
</script>

<style scoped>

</style>
```

## 引用单文件组件

```js
<template>
  <div id="app">
    <!-- 顶部 -->
    <Header></Header>
    <!-- 内容 -->
    <Content></Content>
    <!-- 底部 -->
    <Footer></Footer>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Content from './components/Content.vue'
import Footer from './components/Footer.vue'
export default {
  name: 'App',
  components: { Header, Content, Footer }
}
</script>

<style>
</style>
```

![](/mdImg/单文件组件1.png)

# Mint UI的使用

[官方文档](https://mint-ui.github.io/docs/#/zh-cn2)

## 在脚手架中引入Mint UI

**安装**

```js
npm i mint-ui -S
```

**在main.js中引入**

```js
import Vue from 'vue'
import Mint from 'mint-ui'
import 'mint-ui/lib/style.css'

Vue.use(Mint)
```

# vue router-link-active

[相关文章](https://blog.csdn.net/tanga842428/article/details/53052970)

# css之sticky footer布局

> 完美的css绝对底部

![](/mdImg/elm项目实战7.png)

## html基本结构

```html
<!-- 公告详情页 -->
<div class="detail" v-show='isShowDetail'>
  <div class="detail-wrapper clearfix">
    <div class="detail-main">内容区域，可随机长度</div>
  </div>
  <div class="detail-close">关闭图标</div><!--始终在底部-->
</div>
```

## 基本样式

```css
.detail { // 容器为全屏，所以高度为100%
  position: fixed;
  z-index: 100;
  top: 0;
  left:0;
  width: 100%;
  height: 100%;
  background: rgba(7, 17, 27,0.8);
  overflow: auto; /* 如果内容太长，会显示滚动条查看其余内容*/
  .detail-wrapper {
    min-height: 100%; /* 如果内容不够长时，也保证内容有全屏长度 */
    .detail-main {
      padding-top: 64px;
      padding-bottom: 64px; /*保证内容content区域的底部有为关闭按钮空出的留白*/
    }
  }
  .detail-close {
    position: relative;
    width: 32px;
    height: 32px;
    margin: -64px auto 0 auto; /*让关闭按钮向detail-wrapper里面伸入50px，正好把内容区的50px空白补上*/
    clear: both;
    font-size: 32px;
  }
}
```

![](/mdImg/elm项目实战6.png)

# star组件

[github笔记](https://github.com/BryantOut/starComponent)

# 运用绝对定位初始化视口位置

```css
<style scoped lang='scss'>
.goods {
    position: absolute;
    top:174px;
    bottom:46px;
  	overflow:hidden;
  	width: 100%;
}
</style>
```

![](/mdImg/elm项目实战8.png)

