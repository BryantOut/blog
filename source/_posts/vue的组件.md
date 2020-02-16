---
title: vue的组件
tags: vue
categories: vue
date: 2018-8-30
---

[官网链接](https://cn.vuejs.org/v2/guide/components.html#ad)

# 组件基础

## 基本示例

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
  Vue.component('first',{
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

> **组件是可复用的 Vue 实例，且带有一个名字：**

![](/mdImg/vue组件1.png)

> **因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data` 、 `computed` 、`watch`  、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。**

<!--more-->

## 组件创建模板的细节

> 如果模板结构稍微复杂，你可以考虑使用反引号
>
> **如果你愿意，也可以将模板内容使用单独的结构创建，如template**

```html
<template id='firsttemp'>
  <div>
    <p>你好啊，我的第一个组件456789</p>
    <span>我是来凑热闹的123</span>
  </div>
</template>

<div id="app">
  <first></first>
</div>
<script>
  // 3.还是通过 Vue.component(组件的名称，配置)来创建，
  //   本质也是调用了 Vue.extend -- 语法糖
  Vue.component('first', {
    template: '#firsttemp'
  })
  
  var vm = new Vue({
	el: '#app',
  	data: {

  	}
  })
</script>
```

> template 模板的作用就是用来标识当前组件的 dom 结构的

![](/mdImg/vue组件2.png)

## 组件中的使用变量及指令等

> 组件中的数据是单独存在的，与之前 vm 实例不能混在一起

### **`data`  必须是一个函数**

![](/mdImg/vue组件3.png)

```html
<div id="app">
  <father></father>
</div>
  <template id='father'>
<p>我是父组件{{fatherName}}</p>
</template>
<script>
  // 创建父组件
  Vue.component('father', {
  template: '#father',
  data() {
    return {
      fatherName: '小头爸爸'
    }
  }
})
var vm = new Vue({
  el: '#app',
  data: {

  }
})
</script>
```

# 父子组件

1. 创建组件

```html
<script>
  // 创建父组件
  Vue.component('father', {
    template: '#father',
    data() {
      return {
        fatherName: '小头爸爸'
      }
    },
    // 通过 components 创建子组件
    components: {
      // 创建子组件，并且添加子组件的配置，子组件的配置和父组件完全一样
      son: {
        template: '#son',
        data() {
          return {
            sonName: '大头儿子'
          }
        }
      }
    }
})
var vm = new Vue({
  el: '#app',
  data: {

  }
})
</script>
```

2. 定义模板

```html
<template id='father'>
  <div>
    <p>我是父组件{{fatherName}}</p>
    <!-- 使用子组件，必须在父组件中使用 -->
    <son></son>
  </div>
</template>
<template id='son'>
  <div>
    <p>我是子组件{{sonName}}</p>
  </div>
</template>
```

3. 使用组件

```html
<div id="app">
  <father></father>
</div>
```

# 父组件传递数据给子组件

> 父组件通过属性向子组件传值

![](/mdImg/Vue组件12.png )

完整示例

![](/mdImg/Vue组件13.png)

1. 定义父子组件

```html
<script>
  // 创建父组件
  Vue.component('father', {
    template: '#father',
    data() {
      return {
        sonname: '大头儿子'
      }
    },
    // 通过 component 创建子组件
    components: {
      // 创建子组件，并且添加子组件的配置，子组件的配置和父组件完全一样
      son: {
        template: '#son',
        props: ['myname'],
      }
    }
  })
  var vm = new Vue({
    el: '#app',
  })
</script>
```

## props

> 它是一个数组，作用就是用来接收从父组件中传递来的数据的，里面的值可以是字符串,这些字符串就可以当成data中的属性来使用

2. 准备模板

```html
<template id='father'>
  <div>
    <p>小头爸爸的儿子叫{{sonname}}</p>
    <!-- 使用子组件，必须在父组件中使用 -->
    <!-- 可以使用 v-bind 来动态传递 -->
    <!-- <son v-bind:myname='sonname'></son> -->
    <son :myname='sonname'></son>
  </div>
</template>
<template id='son'>
  <div>
    <p>我叫{{myname}}</p>
  </div>
</template>
```

3. 使用组件

```html
<div id="app">
  <father></father>
</div>
```

## Vue中单向数据流的概念

> 父组件可以向子组件随意传递参数

![](/mdImg/Vue组件13.png)

![](/mdImg/Vue组件14.png)

> 之所以有这个概念，一旦传递过来的不是基础数据类型，而是引用数据类型(obj)，也许该数据还被其他子组件使用，一旦其中一个子组件修改了，会对其他子组件造成影响

**解决**

 ![](/mdImg/Vue组件15.png)

# 子组件传递数据给父组件

> 当点击按钮时，我们需要传递数据给父组件。问题是这个按钮只能做触发

```html
<template id='son'>
  <div>
    <p>女儿的男朋友是：{{myboyfriend}}</p>
    <button @click='tellFather'>告诉老爸女儿的男朋友是</button>
  </div>
</template>
```

> 幸好 Vue 实例提供了一个自定义事件的系统来解决这个问题。可以调用内建的 `$emit`  方法并传入事件的名字，来向父级组件触发一个事件，`emit`  的第二个参数传输值

```html
components: {
  son: {
    template: '#son',
    data() {
    return {
    myboyfriend: '彭于晏'
  	}
  },
  methods: {
    tellFather() {
      // 将数据传递给父组件
      // $emit(事件名称，事件的参数)
      this.$emit('handler',this.myboyfriend)
      }
      }
    }
}
```

> 然后我们可以用 `v-on`  在博文组件上监听这个事件，就像监听一个原生 DOM 事件一样

```html
<template id='father'>
  <div>
    <p>我女儿的男朋友是{{mydaughtersboyfriend}}</p>
    <!-- 使用子组件，必须在父组件中使用 -->
    <son v-on:handler='getName'></son>
  </div>
</template>
```

> 子组件传递数据给父组件可以携带参数

  ![](/mdImg/vue组件16)

# 兄弟组件之间的数据传递

1. 我们先来创建中央事件总线

> eventBus 中我们只有创建了一个新的 Vue 实例，以后它就承担起了组件之间通信的桥梁了，也就是中央事件总线

```html
// 兄弟组件之间的数据传递可以通过总线方式进行传递
// 创建总线--对象
var bus = new Vue()
```

2. 创建一个妹妹组件，引入eventBus这个事件总线，接着添加一个按钮并绑定一个点击事件

```html
<template id="sister">
  <div class="div">
    <p>我是妹妹，我的男朋友叫{{hisname}}</p>
    <button @click='tellMyBrother'>告诉我哥哥我男朋友的名字</button>
  </div>
</template>
<script>
components: {
    sister: {
      template: '#sister',
        data() {
        return {
          hisname: '彭于晏'
        }
      },
        methods: {
          tellMyBrother(){
            bus.$emit('handler',this.hisname)
          }
        }
    }
}
</script>
```

> 我们在响应点击事件的 `tellMyBrother` 函数中用 `$emit` 触发了一个自定义的 `handler` 事件，并传递了一个字符串参数。
>
> `$emit` 实例方法触发当前实例（这里的当前实例就是eventBus）上的事件，附加参数都会传给监听器回调

3. 我们再创建一个哥哥组件，引入eventBus事件总线，并用一个标签来显示传递过来的值

```html
<template id="brother">
  <p>我妹妹的男朋友是{{hisname}}</p>
</template>
<script>
  components: {
    brothder: {
      template: '#brother',
        data() {
        return {
          hisname: '***'
        }
      },
        mounted(){
          // 一开始就监听妹妹组件是否发射了事件
          // $on 可以用于监听事件，第一个参数是事件名称，第二个参数是事件处理函数
          bus.$on('handler',(data)=>{
            console.log(data)
            this.hisname = data
          })
        }
    }
  }
</script>
```

- 我们在 `mouted` 中，监听了 `handler` ，并把传递过来的字符串参数传递给监听器的回调函数。
- `mouted`:是一个 Vue 生命周期中的钩子函数，简单点说就类似于 jQuery 的 ready，Vue 会在文档加载完毕后调用 `mouted` 函数
- `$on:` 监听当前实例的自定义事件（此处当前实例为eventBus）。事件可以由 `$emit` 触发，回调函数会接收所有传入事件触发函数（$emit）的额外参数。 

4. 在父组件中，注册这两个组件，并添加这个组件的标签

```html
<div id="app">
  <father></father>
</div>
<template id="father">
  <div>
    <p>我是父组件</p>
    <brothder></brothder>
    <sister></sister>
  </div>
</template>
```

![](/mdImg/vue组件4.png)

## 总结：

1. 创建一个事件总线，例如示例中的 `eventBus`，用它作为通信桥梁
2. 在需要传值的组件中用 `eventBus.$emit`  触发一个自定义事件，并传递参数
3. 在需要接收数据的组件中用 `eventBus.$on` 监听自定义事件，并在回调函数中处理传递过来的参数

# 另外：

1. 兄妹组件之间与父子组件之间的数据交互，两者相比较，兄弟组件之间的通信其实和子组件向父组件传值有些类似，其实他们的通信原理都是相同的，例如子向父传值也是 `$emit` 和 `$on` 的形式，只是没有 `eventBus` ，但若我们仔细想想，此时父组件其实就充当了 `eventBus` 这个事件总线的角色
2. 这种用一个 Vue 实例来作为中央事件总线来管理组件通信的方法只适用于通信需求简单一点的项目，对于更复杂的情况，Vue 也由提供更复杂的状态管理模式 Vuex 来进行处理。

# Vue的动态组件

> 通过使用保留的 `<component>`  元素，动态地绑定到它的 `is`  特性，我们让多个组件可以使用同一个挂载并动态切换。根据 `v-bind:is="组件名"`  中的组件名去自动匹配组件，如果匹配不到则不显示。

```html
<div id="app">
  <ul>
    <li>
      <a href="javascript:;" @click='currentPage = "index"'>鸣人</a>
    </li>
    <li>
      <a href="javascript:;" @click='currentPage = "catagore"'>小樱</a>
    </li>
	……
  </ul>
  <component v-bind:is='currentPage'></component>
</div>
<script>
  // 组件的创建：不是写在 vm 实例中
  // 1. 还是通过 Vue.component (组件名称，配置)来创建，
  Vue.component('index', {
    // 2.template 模板的作用就是用来标识当前组件的 dom 结构
    template: `<img src="./images/1.png" alt="">`
  })
  Vue.component('catagore', {
    template: `<img src="./images/2.png" alt="">`
  })
  ……
  var vm = new Vue({
    el: '#app',
    data: {
      currentPage: "index"
    }
  })
</script>
```

![](/mdImg/vue组件5.png)

**改变挂载的组件，只需要修改 `is`  指令的值即可。**

# 使用 vue文件创建 vue 组件

[知识点回顾之vue单文件组件](https://bryantout.github.io/2018/09/03/vue%E5%8D%95%E6%96%87%E4%BB%B6%E7%BB%84%E4%BB%B6/)

**template.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <!-- <script src="./dist/main.js"></script> -->
</head>
<body>
    <!--要在页面上显示的内容-->
    <div id="app">
        
    </div>
</body>
</html>
```

**app.js**

```html
import Vue from 'vue';
import HelloWorld from './component/HelloWorld.vue';

new Vue({
    el: '#app',
    render: fn => {
        return fn(HelloWorld)
    }
})
```

**HelloWorld.vue**

```html
<template>
    <div>{{msg}}</div>
</template>

<script>    
  	// default一般是用于返回默认的匿名实例
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
