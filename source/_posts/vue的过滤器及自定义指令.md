---
\title: vue的过滤器【时间格式化】
tags: vue
categories: vue
date: 2018-8-28
---

# 全局过滤器

```js
// 全局过滤器的定义格式
Vue.filter(过滤器名称，处理函数)
```

> 可以用全局方法 Vue.filter() 注册一个全局自定义过滤器，它接收两个参数：过滤器 ID 和过滤器函数。过滤器函数以值为参数，返回转换后的值

<!--more-->

```js
<script>
Vue.filter('timeFormat', (time, spe) => {
  time = new Date(time)
  // 获取数据中的年月日
  var y = time.getFullYear()
  var m = time.getMonth() + 1
  var d = time.getDate()
  // 过滤器一般都会有返回值
  return y+spe+m+spe+d
})
</script>
```

```js
<!-- 使用过滤器：在你想过滤的数据后面添加管道符  源数据 | 过滤器的名称 -->
<!-- 过滤器传递参数：你就把过滤器当成函数 -->
<!-- 就算传递的自定义参数，也不会影响之前的默认参数的传递 -->
<td>{{value.createdTime | timeFormat('/')}}</td>
```

![](/mdImg/vue的过滤器.png)



# 私有过滤器

> 定义在 VM中的filters对象中的所有过滤器都是私有过滤器

```js
new Vue({
  el:'#app',
  filters:{        
      '过滤器名称':function(管道符号|左边对象的值,参数1,参数2,....) {
        return 对管道符号|左边参数的值做处理以后的值
      })    
  }
});
```

> 调用方法

```js
//Vue2.0 调用过滤器传参写法：
<span>{{ msg | 过滤器id('参数1','参数2' ....) }}</span>
```

> 【应用示例】自定义私有过滤器实现日期格式化

```html
<script> 
/*-1、 定义私有的日期格式化过滤器：*/
  new Vue({
    el:'#app',
    data:{
      time:new Date()
    },
    filters:{
      //定义在 VM中的filters对象中的所有过滤器都是私有过滤器
      datefmt:function(input,splicchar){
        var date = new Date(input);
        var year = date.getFullYear();
        var m = date.getMonth() + 1;
        var d = date.getDate();            
        var fmtStr = year+splicchar+m +splicchar+d;
        return fmtStr; //返回输出结果
      }
    }
  });
</script>
<!--2、使用-->
<div id="app">
  {{ time | datefmt('-') }} //Vue2.0传参写法
</div>
```

# 自定义指令

> 丰富的内置指令能满足我们的绝大部分业务需求，不过在需要的一些特殊功能时，我们仍然希望对DOM进行底层的操作，这时就要用到自定义指令。

```js
// 全局注册
Vue.directive('focus',{
  // 指令选项
})

// 局部注册
var app = new Vue({
  el: '#app',
  directives:{
    focus:{
      // 指令选项
    }
  }
})
```

> 写法与组件基本类似，只是方法名由 compontent 改为 directive。上例只是注册了自定义指令 v-focus ，还没有实现具体功能，下面具体介绍自定义指令的各个选项。

## 自定义指令的钩子函数

> 自定义指令的选项是由几个钩子函数组成的，每个都是可选的。

| 名称               | 描述                                       |
| ---------------- | ---------------------------------------- |
| bind             | 只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。 |
| inserted         | 被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于document中）。 |
| update           | 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。 |
| componentUpdated | 被绑定元素所在模板完成一次更新周期时调用。                    |
| unbind           | 只调用一次，指令与元素解绑时调用。                        |

> 在大多数使用场景，我们会在 bind 钩子里绑定一些事件，比如在 docuemnt 上用  addEventListener 绑定，在 unbind 里用 removeEventListener 解绑。

### 例子 v-focus

> 可以根据需求在不同的钩子函数内完成逻辑代码，例如上面的 **v-focus** ，我们希望在元素插入父节点时调用，那用到的是 **inserted** 。示例代码如下：

```html
<div id="app">
  <input type="text" v-focus>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  Vue.directive('focus',{
    inserted:function(el){
      // 聚焦元素
      el.focus()
    }
  })

  var app = new Vue({
    el: '#app'
  })
</script>
```

### 钩子函数的参数

> 每个钩子函数都有几个参数可用，比如上面我们用到了el。它们的含义如下：

| 名称       | 描述                                     |
| -------- | -------------------------------------- |
| el       | 指令所绑定的元素，可以用来直接操作 DOM                  |
| binding  | 一个对象，包含以下属性：                           |
| vnode    | Vue编译生成的虚拟节点                           |
| oldVnode | 上一个虚拟节点仅在update和componentUpdated钩子中可用。 |

#### binding对象包含的参数

| 名称         | 描述                                       |
| ---------- | ---------------------------------------- |
| name       | 指令名，不包含v-前缀                              |
| value      | 指令的绑定值，例如v-my-directive="1+1"，value的值是2  |
| oldValue   | 指令绑定的前一个值，仅在update和componentUpdated钩子中使用。无论值是否改变都可用。 |
| expression | 绑定值的字符串形式。例如v-my-directive="1+1"，expression的值是"1+1" |
| arg        | 传给指令的参数。例如 v-my-directive:foo,arg的值是foo  |
| modifiers  | 一个包含修饰符的对象。例如 v-my-directive.foo.bar，修饰符对象modifiers的值是{foo:true,bar:true} |

