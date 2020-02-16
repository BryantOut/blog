---
title: 本地存储,前后端数据传输
tags: local-storage\前后端数据传输
categories: local-storage\前后端数据传输
date: 2018-9-7
---

# local-storage

# 本地存储

在h5中，本地存储由两个，一个是永久存储`localstorage`和会话存储`sessionStorage`

他们可以把一些数据存储在浏览器

他们的应用场景大概是有，**记住用户信息**，**记住阅读历史**等等

**我们来看一下他们两个和cookie的区别(面试题重点)**

| 区别        | sesstionStorage | localstorage | cookie |
| --------- | --------------- | ------------ | ------ |
| 存储量       | 5M(具体看浏览器版本)    | 5M(具体看浏览器版本) | 4KB    |
| 存放地点      | 浏览器             | 浏览器          | 浏览器    |
| 发送http请求时 | 不会带上            | 不会带上         | 会带上    |
| 生命周期      | 浏览器关闭后不存在       | 永久保留（*）      | 过期日期   |
| 访问权限      | 同源网站            | 同源网站         | 同源网站   |

<!--more-->

## 语法

本地存储是以键值对的方式放在浏览器中，可以在谷歌浏览器里面进行查看

![](./mdImg/local-strong1.png)

localstrong 和 sessionStorage两者的api都是一样的。按照键值对的方式操作

- setItem(key,val)存入

```js
//存入
localStorage.setItem("movice","当幸福来敲门");
```

![](./mdImg/localStorage1.png)

- getItem(key) 获取

```js
// 获取
 var val=localStorage.getItem("movie")//  当幸福来敲门
```

- removeItem(key) 删除

```js
// 删除
localStorage.removeItem("movie")  
// 获取
var val=localStorage.getItem("movie")//  为 undefined
```

- clear()  清空   清空整个本地存储

```js
// 清空 
localStorage.clear();
```

## 重点注意

​	当存入数据时，浏览器都会自动先调用该数据的**toString()**方法，因此

- 不管存入的是什么类型的数据，都会变成字符串类型

```js
// 存入 bool 
localStorage.setItem("test",true);
var test=localStorage.getItem("test");
console.log(typeof test);// string 类型
```

- 当存入复杂类型（数组，对象）时，需要先转为json字符串，再存入，否则数据丢失

  - 使用**JSON.stringify** 将对象转为**json字符串**
  - **错误做法**

  ```js
  // 复杂类型
  var obj={name:"小朱"}
  localStorage.setItem("test",obj)
  console.log(localStorage.getItem("test")); //  [object Object]
  ```

  - 正确做法

  ```js
  // 复杂类型
  var obj={name:"小朱"}
  // 将对象转为json格式
  var jsonObj=JSON.stringify(obj);
  // 存入
  localStorage.setItem("test",jsonObj)
  // 类型是字符串
  console.log(typeof localStorage.getItem("test"));// string
  // 数据成功存入 
  console.log(localStorage.getItem("test"));// {"name":"小朱"}

  ```

- 那么取出数据的时候，当然也要解析字符串

  - 使用JSON.parse将json字符串转为对象

```js
var obj={name:"小朱"}
// 将对象转为json格式
var jsonObj=JSON.stringify(obj);
localStorage.setItem("test",jsonObj)
// 取出数据
var newJson=localStorage.getItem("test");
// json转为对象
var newObj=JSON.parse(newJson);
console.log(typeof newObj);// object 类型 
```

# 复习json

##  JSON简介

1.1  与 XML 不同之处

- 没有结束标签
- 更短
- 读写的速度更快
- 能够使用内建的 **javaScript eval()** 方法进行解析
- 使用数组
- 不使用保留字

1.2  为什么使用 JSON？

对于 AJAX 应用程序来说，JSON 比 XML 更快更易使用：

**使用 XML**

- 读取 XML 文档
- 使用 XML DOM 来循环遍历文档
- 读取值并存储在变量中

**使用 JSON** 

- 读取 **JSON** 字符串
- 用 **eval()** 处理 **JSON** 字符串



## JSON.stringify()--客户端

JSON 通常用于与服务器交换数据。

在向服务器发送数据时一般是字符串。

我们可以使用 **JSON.stringify()**方法将JavaScript对象转换为字符串

### 语法

```js
JSON.stringify(value[, replacer[, space]])
```

参数说法：

- value:
  - 必须，一个有效的JSON对象

### 用法：

#### JavaScript 对象转换

例如我们向服务器发送以下数据：

```js
var obj = { "name":"runoob", "alexa":10000, "site":"www.runoob.com"};
```

我们使用 JSON.stringify() 方法处理以上数据，将其转换为字符串：

```js
var myJSON = JSON.stringify(obj);
```

myJSON 为字符串。

我们可以将myJSON发送到服务器

```js
实例：
var obj = { "name":"runoob", "alexa":10000, "site":"www.runoob.com"};
var myJSON = JSON.stringify(obj);
document.getElementById("demo").innerHTML = myJSON;
```

#### JavaScript 数组转换.

我们也可以将**JavaScript** 数组转换为**JSON**字符串：

```js
var arr = [ "Google", "Runoob", "Taobao", "Facebook" ];
var myJSON = JSON.stringify(arr);
```

myJSON为字符串。

我们可以将 myJSON 发送到服务器:

```js
var arr = [ "Google", "Runoob", "Taobao", "Facebook" ];
var myJSON = JSON.stringify(arr);
document.getElementById("demo").innerHTML = myJSON;
```

##  JSON.parse()--客户端

JSON 通常用于与服务端交换数据。

在接收服务器数据时一般是字符串。

我们可以使用 JSON.parse() 方法将数据转换为 JavaScript 对象

### 语法

```js
JSON.parse(text[, reviver])
```

参数说明：

- text:必须，一个有效的 JSON 字符串。

### JSON 解析实例

例如我们从服务器接收了以下数据：

```js
{ "name":"runoob", "alexa":10000, "site":"www.runoob.com" }
```

我们使用 JSON.parse() 方法处理以上数据，将其转换为 JavaScript 对象:

```js
var obj = JSON.parse('{ "name":"runoob", "alexa":10000, "site":"www.runoob.com" }');
```

```html
解析前要确保你的数据是标准的 JSON 格式，否则会解析出错。

你可以使用我们的在线工具检测：https://c.runoob.com/front-end/53。
```

解析完成后，我们就可以在网页上使用 JSON 数据了：

```js
<p id="demo"></p>
 
<script>
var obj = JSON.parse('{ "name":"runoob", "alexa":10000, "site":"www.runoob.com" }');
document.getElementById("demo").innerHTML = obj.name + "：" + obj.site;
</script>
```

#### 服务端接收 JSON 数据

我们可以使用 AJAX 数据从服务器请求 JSON 数据，并解析为 JavaScript 对象。

```js
var xmlhttp = new XMLHttpRequest();
xmlhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
        myObj = JSON.parse(this.responseText);
        document.getElementById("demo").innerHTML = myObj.name;
    }
};
xmlhttp.open("GET", "/try/ajax/json_demo.txt", true);
xmlhttp.send();
```

#### 从服务器端接收数组的 JSON 数据

如果从服务器接收的是数组的 JSON 数据，则 JSON parse 会将其转换为 JavaScript 数组：

```js
var xmlhttp = new XMLHttpRequest();
xmlhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
        myArr = JSON.parse(this.responseText);
        document.getElementById("demo").innerHTML = myArr[1];
    }
};
xmlhttp.open("GET", "/try/ajax/json_demo_array.txt", true);
xmlhttp.send();
```

##  json_decode()--服务器端

### 描述

对JSON格式的字符串进行解码，转换为 PHP 变量

### 语法

```php
json_decode ($json_string [,$assoc = false [, $depth = 512 [, $options = 0 ]]])
```

### 参数

-  **json_string** 待解码的 JSON 字符串，必须是UTF-8编码数据
-  **assoc** 当参数为TRUR时，将返回数组，FALSE时返回对象

### 实例

```php
<?php
   $json = '{"a":1,"b":2,"c":3,"d":4,"e":5}';

   var_dump(json_decode($json));
   var_dump(json_decode($json, true));
?>
```

以上代码执行结果为：

```php
object(stdClass)#1 (5) {
    ["a"] => int(1)
    ["b"] => int(2)
    ["c"] => int(3)
    ["d"] => int(4)
    ["e"] => int(5)
}

array(5) {
    ["a"] => int(1)
    ["b"] => int(2)
    ["c"] => int(3)
    ["d"] => int(4)
    ["e"] => int(5)
}
```

## json_encode()--服务器端

### 描述

PHP json_encode() 用于对变量进行 JSON 编码，该函数如果执行成功返回 JSON 数据，否则返回 FALSE。

### 语法

```php
string json_encode ( $value [, $options = 0 ] )
```

### 参数

- **value** 要编码的值，该函数只对 UTF-8 编码的数据有效



### 实例

以下实例演示了如何将PHP数组转换为JSON格式的数据：

```php
<?php
   $arr = array('a' => 1, 'b' => 2, 'c' => 3, 'd' => 4, 'e' => 5);
   echo json_encode($arr);
?>
```

以上代码执行结果为：

```php
{"a":1,"b":2,"c":3,"d":4,"e":5}
```





