---
title: vue单文件组件
tags: vue
categories: vue
date: 2018-9-3
---

# 介绍

在很多 Vue 项目中，我们使用 `Vue.component` 来定义全局组件，紧接着用 `new Vue({el: '#app'})`  在每个页面内指定一个容器元素。

<!--more-->

```html
<div id="app">
  <!-- 使用组件：就像普通的标签一样使用 -->
  <first></first>
</div>
<script>
  // 组件的创建：不是写在 vm 实例中
  // 1.通过 Vue.extend 和 Vue.component

  // 3.还是通过 Vue.component (组件名称，配置)来创建，
  //   本质上也是调用了 Vue.extend --语法糖
  Vue.component('firstCom',{
    // template 模板的作用就是用来标识当前组件的 dom 结构
    template:'<p>你好啊，我的第一个组件</p>'
  })

  var vm = new Vue({
	el: '#app',
  	data: {

  	}
  })
</script>
```

缺点：

1. 全局定义：强制要求每个 component 中的命名不得重复。
2. 字符串模板：缺乏语法高亮，在 HTML 有多行的时候，需要用到丑陋的 `\`
3. 不支持 CSS：意味着当 HTML 和 JavaScript 组件化时， CSS 明显被遗漏
4. 没有构建步骤：限制只能使用 HTML 和 ES5 JavaScript ，而不能使用预处理器，如 Pug (formerly Jade) 和 Babel

> 文件扩展名为 `.vue` 的 **single-file components(单文件组件)** 为以上所有问题提供了解决方法，并且还可以使用 webpack 或 Browserify 等构建工具。

这是一个文件名为 `HelloWorld.vue` 的简单实例

```html
<template>
    <div>{{msg}}</div>
</template>

<script>    
    export default {
        data(){
            return {
                msg:'Hello World'
            }
        }
    }  
</script>

<style>
    div {
        color : red;
    }
</style>   
```

# 怎么看待关注点分离？

一个重要的事情值得注意，**关注点分离不等于文件类型分离。**在现代 UI 开发中，我们已经发现相比于把代码库分离成三个大的层次并将其相互交织起来，把它们划分为松散耦合的组件再将其组合起来更合理一些。在一个组件里，其模板、逻辑和样式是内部耦合的，并且把他们搭配在一起实际上使得组件更加内聚且更可维护。

# 项目实践

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