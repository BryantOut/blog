---
title: vue的生命周期及钩子函数
tags: vue
categories: vue
date: 2018-8-28
---

![](/mdImg/lifecycle.png)



| 参数            | 描述                                       |
| ------------- | ---------------------------------------- |
| beforecreated | el 和 data 并未初始化                          |
| created       | 完成了 data 数据的初始化， el 没有                   |
| beforeMount   | 完成了 el 和 data 初始化                        |
| mounted       | 完成挂载【**页面加载完毕之后就会自动执行的函数.相当于window.onload**】 |

![](/mdImg/vue生命周期2.png)

<!--more-->

> 另外在标红处，我们能发现el还是 {{message}}，这里就是应用的 `Virtual DOM`（虚拟Dom）技术，先把坑占住了。到后面`mounted`挂载的时候再把值渲染进去。

```html
<div id="app">
  <input type="text" ref='goodsId'>
    </div>
<script>
    var vm = new Vue({
      el: '#app',
      data: {

      },
      // mounted:页面加载完毕之后就会自动执行的函数.相当于window.onload
      // 生命周期的内容
      mounted() {
        // 在vue中，可以通过$refs来获取之前通过ref标识的dom成员
        // $refs是当前vm实例的内置成员
        // this.$refs是一个对象，里面就存储着之前所有通过ref设置标识的dom元素
        this.$refs.goodsId.focus()
      }
    })
</script>
```

![](/mdImg/钩子函数1.png)

| 事件            | 描述                                       |
| ------------- | ---------------------------------------- |
| beforeUpdate  | 数据变更导致虚拟DOM重新渲染之前调用                      |
| updated       | 数据变更导致虚拟DOM重新渲染之后调用                      |
| beforeDestroy | 实例销毁之前调用，在这一步，实例完全可用                     |
| destroyed     | vue实例指向的所有东西解除绑定，包括watcher、事件、所以的子组件，后续就不再受vue实例控制了 |

