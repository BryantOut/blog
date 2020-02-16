---
title: vue中的指令
tags: es6新特性
categories: es6新特性
date: 2018-8-27
---

# # v-text

> v-text 可以将一段文本渲染到指定的元素中，例如

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        ……
        <script src="./js/vue2.js"></script>
    </head>
    <body>
        <div id="app">
            <p v-text='msg'></p>
        </div>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    msg:'科比布莱恩特'
                }
            })
        </script>
    </body>
</html>
```

<!--more-->

# # v-html

> 在网站上动态渲染任意 HTML 是非常危险的，因为容易导致 [XSS 攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。只在可信内容上使用 `v-html`，**永不**用在用户提交的内容上。

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        ……
        <script src="./js/vue2.js"></script>
    </head>
    <body>
        <!-- 插值表达式和v-text将数据解释为纯文本，而非 HTML
        为了输出真正的 HTML ,你需要使用 v-html 指令 -->
        <div id="app" v-html='msg'></div>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    msg:'<h1>科比布莱恩特</h1>'
                }
            })
        </script>
    </body>
</html>
```

# # v-show

> 根据表达式之真假值，切换元素的 `display` CSS 属性。**当条件变化时该指令触发过渡效果。**
>

# # v-if

> 根据表达式的值的真假条件渲染元素。在切换时元素及它的数据绑定 / 组件被销毁并重建。如果元素是 `<template>` ，将提出它的内容作为条件块。

```html
<!-- 使用建议：
  1.如果是在大量操作 dom ,建议使用 v-show 来实现，避免反复创建 dom
  2.如果是异步操作，建议使用 v-if,避免解析不必要的 dom 结构
 -->
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        ……
        <script src="./js/vue2.js"></script>
    </head>
    <body>
        <div id="app">
            <!-- v-if:通过控制 dom 元素的创建来实现显示和隐藏 -->
            <p v-if = 'isShow'>我是通过v-if来控制的</p>
            <!-- 通过控制元素的样式来实现显示和隐藏 -->
            <p v-show = 'isShow'>我是通过v-show来控制的</p>
            <button @click='isShow = !isShow'>切换</button>
        </div>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    isShow:true
                }
            })
        </script>
    </body>
</html>
```

# # v-if、v-else-if、v-else

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        ……
        <script src="./js/vue2.js"></script>
    </head>
    <body>
        <div id="app">
            <p v-if='score>=90'>A</p>
            <p v-else-if='score>=80'>B</p>
            <p v-else-if='score>=70'>C</p>
            <p v-else-if='score>=60'>D</p>
            <p v-else>D</p>
        </div>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    score:0
                }
            })
        </script>
    </body>
</html>
```

# # v-for

> 基于源数据多次渲染元素或模板块。此指令之值，必须使用特定语法 `alias in expression`，为当前遍历的元素提供别名：

```html
 <!-- 你想循环生成哪个结构，就在这个结构中添加v-for -->
 <!-- 一定要添加一个key，这样在数据更新的时候可以提高刷新视图的效率 -->
```

**例一**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    ……
    <script src="./js/vue2.js"></script>
</head>

<body>   
    <div id="app">
        <ul>
            <!-- 遍历数组 -->
            <li v-for='(value,index) in arr' :key='index'>{{value +"~~~"+ index}}</li>
        </ul>
        <!-- 遍历对象 -->
        <ul>
            <li v-for='(value,key,index) in obj' :key='key'>{{value +"----" + key +"----" + index}}</li>
        </ul>
    </div>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                arr: [1, 3, 5, 7, 9, 11, 13],
                obj: {
                    name: 'bryantout',
                    age: 22,
                    address: '潮州市体育馆',
                    hobby: '打篮球'
                }
            }

        })
    </script>
</body>

</html>
```

**例二**

```html
<el-select v-model="roleFrom.role_name" placeholder="请选择活动区域">
  <el-option v-for='item in AllRoleList' :key='item.id' :label="item.roleName" :value="item.id"></el-option>
</el-select>
```

![](/mdImg/v-for.png)

# # v-on

```html
<!-- 
  v-on:
    1. 作用：绑定事件监听，表达式可以是一个方法的名字或一个内联语句。
    如果没有修饰符也可以省略，用在普通的 html 元素上时，只能监听原生
    DOM 事件。用在自定义元素组件上时，也可以监听组件触发的自定义事件。

  2、常用事件：
    v-on:click
    v-on:keydown
    v-on:keyup
    v-on:mousedown
    v-on:mouseover
    v-on:submit
    ....
-->
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    ……
    <script src="./js/vue2.js"></script>
</head>
<body>
    
    <div id="app">
        <!-- 方法处理器 -->
        <button v-on:click='doThis'>奇拉比先生喜欢谁</button>
        <!-- 缩写 -->
        <button @click='doThis'>奇拉比先生喜欢谁</button>
        <!-- 阻止默认行为 -->
        <a href="https://baidu.com" @click.prevent='alert'>阻止默认行为</a>
        <!-- 键修饰符，键别名 -->
        <input type="text" @keyup.enter='onEnter' v-model='username'>
    </div>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                msg: '洛杉矶湖人',
                username:''
            },
            methods: {
                doThis() {
                    console.log(this.username)
                },
                alert() {
                    alert(this.msg)
                },
                onEnter() {
                    alert(this.username)
                }
            }
        })
    </script>
</body>

</html>
```

# # v-bind

> 作用：可以给 html 元素或者组件动态地绑定一个或多个特性【**例如动态绑定 style 和 class**】

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        ……
        <script src="./js/vue2.js"></script>
        <style>
            .redfont{
                color: red
            }
            .blueFont {
                color:#0094FF
            }
            .hasUnder {
                text-decoration: underline
            }
        </style>
    </head>
    <body>
        <div id="app">
            <p>{{msg}}</p>
            <!-- v-bind:可以为后面得任意属性动态地绑定值 -->
            <img v-bind:src="src" alt="">
            <!-- 简写 -->
            <img v-bind:src="src2" alt="">
            <!-- 动态绑定的时候拼接字符串 -->
            <a :href="'/del?id='+id">删除</a>
            <!-- 动态绑定样式：class='{样式名称：值}' -->
            <p :class="{redfont:hasClass}">我有样式</p>
            <!-- 通过[]绑定多个样式 -->
            <p class="redfont hasUnder">我有多个样式</p>
            <p :class=[redfont,hasUnder]>我有多个样式</p>
            <p :class=[hasUnder,{redfont:hasClass}]>我有多个样式</p>
        </div>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    msg:'显示下面的图片',
                    name:'Bryant',
                    src:'./images/timg (9).jpg',
                    src2:'./images/timgdfdf.jpg',
                    id:2,
                    hasClass:false,
                    setColor:'blueFont',
                    setUnder:'hasUnder'
                }
            })
        </script>
    </body>
</html>
```

# # v-model

> 在表单控件或者组件上创建双向绑定。

```html
<!-- 
  v-model:
  1.在表单控件或者组件上创建双向绑定
  2.v-model 仅能在如下元素中使用
    input
    select
    textarea
    components（Vue中的组件）
  3.举例
-->
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        ……
        <script src="./js/vue2.js"></script>
    </head>
    <body>
        <div id="app">
            <p>{{username}}</p>
            <input type="text" v-model='username'>
        </div>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    username:'科比布莱恩特'
                }
            })
        </script>
    </body>
</html>
```

# # v-cloak

> 这个指令保持在元素上直到关联实例结束编译。

```html
<-- 和 CSS 规则如 { display:none } 一起用时，
            这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。-->
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        ……
        <script src="./js/vue2.js"></script>
        <style>
            /* 这个指令保持在元素上直到关联实例结束编译。
            和 CSS 规则如 { display:none } 一起用时，
            这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。 */
            [v-cloak] {
                display: none;
            }
        </style>
    </head>
    <body>
        <div id="app">
            <p v-cloak>{{msg}}</p>
        </div>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    msg:'中华人民共和国'
                }
            })
        </script>
    </body>
</html>
```

# # 自定义指令

> 定义的位置在 vm 实例之外--全局自定义指令

```javascript
// 定义方式：
Vue.directive(名称，{指令的配置}) 
// 名字最好都用小写，假如驼峰命名的话  setFocus/v-set-focus
```

## ## 什么是钩子函数？

> 钩子实际上是一个处理消息的程序段，通过系统调用，把它挂入系统。每当特定的消息发出，在没有到达目的窗口前，钩子程序就先捕获该消息，亦即钩子函数先得到控制权。这时钩子函数即可以加工处理（改变）该消息，也可以不作处理而继续传递该消息，还可以强制结束消息的传递。对每种类型的钩子由系统来维护一个钩子链，最近安装的钩子放在链的开始，而最先安装的钩子放在最后，也就是后加入的先获得控制权。

## ##  自定义指令提供几个钩子函数可选

| 参数               | 描述                                       |
| ---------------- | ---------------------------------------- |
| bind             | 只调用一次，指令第一次绑定到元素时调用，<br />用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。 |
| inserted         | 被绑定元素插入父节点时调用<br />（父节点存在即可调用，不必存在于 document 中）。 |
| update           | 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新 |
| componentUpdated | 被绑定元素所在模板完成一次更新周期时调用。                    |
| unbind           | 只调用一次， 指令与元素解绑时调用。                       |

## ## 实例

```js
<script>
Vue.directive('myfocus', {
  inserted(el) {
    el.focus()
  }
})
</script>
```

### ### inserted的三个参数

| 参数      | 描述               |
| ------- | ---------------- |
| el      | 当前使用这个指令的dom元素   |
| binding | 指令默认传递的数据所在的对象   |
| vnode   | 当前添加这个指令的dom节点对象 |

```html
<input type="text" v-model='id' v-myfocus>
```

## ## 实例2

```html
<div id="app">
  <p v-bind-own-color='pinkColor'>科比布莱恩特</p>
</div>
<script>
    // 1. Vue.directive(名称，{指令的配置}) 
    Vue.directive('bindOwnColor', {
    inserted(el,binding,vnode) {
     	el.style.color = binding.value
    }
  })
var vm = new Vue({
  el: '#app',
  data: {
    pinkColor:'pink'
  }
})
</script>
```

![](/mdImg/自定义指令.png)