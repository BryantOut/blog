---
title: vue的computed和watch
tags: vue
categories: vue
date: 2018-8-28
---



# computed

> 计算属性出现的目的是解决模板中放入过多的逻辑会让模板过重且难以维护的问题；计算属性是基于它们的依赖进行缓存的

<!--more-->

```html
<div id="app">
  <input type="text" v-model='firstName'><br>
  <input type="text" v-model='secondName'><br>
  <input type="text" v-model='getFullName'>
  <!-- <p>{{getFullName()}}</p> -->
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      firstName: 'Bryant',
      secondName: 'Kobe',
    },
    // 添加计算属性
    // 这个属性中的成员可以像 Data 中的成员一样使用，
    computed: {
      getFullName() {
        return this.firstName + ":" + this.secondName
      }
    }
</script>
```

> **这个属性中的成员可以像 Data 中的成员一样使用**

# watch

> 侦听器（watch）用来观察和响应 Vue 实例上的数据变动

```html
<div id="app">
  <input type="text" v-model='firstName'>
  <input type="text" v-model='FamilyName'>
  <p>名字叫：{{fullName}}</p>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      firstName: '嘉华',
      FamilyName: '谢',
      fullName: ''
    },
    // watch：监听（侦听）属性
    watch: {
      // 创建监听函数：这个函数的名称不能随便，它的名称就是你想监听的data中的属性名称
      // 这些函数有两个参数，分别是 newval(新值), oldval(旧值)
      // 如果函数名称对应的属性值有变化，那么就会自动的触发这个监听函数
      firstName(newval,oldval){
        this.fullName = newval + this.FamilyName
      } ,  
      FamilyName(newval,oldval){
        this.fullName = newval + this.firstName
      } 
    }
  })
</script>
```

![](/mdImg/watch.png)

# 比较

> 注意：通常情况下用computed，当需要在数据变化时**执行异步**或**开销较大**的操作时，用watch

# watch深度监听

```html
<div id="app">
  <input type="text" placeholder="请输入年龄" v-model='obj.age'>
  <span v-show='isshow'>年龄不合法</span>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      obj:{
        age:''
      },
      isshow:false
    },
    watch:{
      // 如果只想监听基本个属性值的变化，我们就可以这么写：
      'obj.age'(){
        if (this.obj.age<=130&&this.obj.age>=0) {
          this.isshow = false
        } else {
          this.isshow = true
        }
      }
    }
  })
</script>
```

# Vue.js 中 computer 和 methods 不同机制

> 在 Vue.js 中，有 methods 和 computed 两种方式来动态当作方法来用的

1. 首先最明显的不同就是调用的时候，**methods 要加上()**
2. 我们可以使用 methods 来替换 computed，效果上两个都一样的，但是 **computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值**。而使用 **methods ，在重新渲染的时候，函数总会重新调用执行**

为了方便理解，先上一段源码

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>title</title>
        <script src="https://cdn.bootcss.com/vue/2.4.2/vue.min.js"></script>
    </head>

    <body>
        <div class="test">    <!--computed计算属性-->
            <p>{{now}}</p>
            <p>{{now}}</p>
            <p>{{now}}</p>
            <p>{{now}}</p>
            <hr />            <!--横线分割-->
</div>
        <div class="test2">   <!--methods方法，注意new（）加了括号-->
            <p>{{now()}}</p>
            <p>{{now()}}</p>
            <p>{{now()}}</p>
            <p>{{now()}}</p>
        </div>
    </body>

    <script type="text/javascript">
        var myVue = new Vue({
            el: ".test",
            computed: {
                now: function() {
                    var yanshi = 0;
                    for(var o = 0; o < 2000; o++) {     //延时
                        for(var q = 0; q < 2000; q++) {
                            yanshi++;
                        }
                    }
                    return Date.now()
                }
            }
        });
        var vue2 = new Vue({
            el: '.test2',
            methods: {
                now: function() {
                    var yanshi = 0;
                    for(var o = 0; o < 2000; o++) {
                        for(var q = 0; q < 2000; q++) {
                            yanshi++;
                        }
                    }
                    return Date.now()
                }
            }
        })
    </script>

</html>
```

![](/mdImg/vue的computed和watch的机制区别.png)

1. 运行结果如上，可以看出 `computed ` 计算属性的话，每次进入页面将一直沿用第一次的信息，不会在再触发 `now `，这就是依赖缓存（有延时的情况下多次输出的时间相同）
2. 而 `methods `是实时的，在重新渲染时，函数总会重新调用执行，不会缓存，（多次输出时间不同）
3. 可以说使用 `computed ` 性能会更好，但是如果你不希望缓存，你可以使用 `methods `属性。
4. `computed` 属性默认只有 `getter`，不过在需要时你也可以提供 `setter`：所以其实 `computed `也是可以传参的。