---
title: 微信小程序学习
tags: 微信小程序
categories: 微信小程序
date: 2018-9-21
---

## 小程序的特点

- 小程序适合做简单的、用完即走的应用
- 小程序适合低频的应用
- 小程序适合性能要求不高的应用

<!--more-->

## 什么是小程序

![](/mdImg/smallProgram.png)

张小龙这样定义：

- 不需要下载安装即可使用
- 用户“用完即走”，不用关心是否安装太多应用
- 应用无处不在，随时可用

![](/mdImg/smallProgram1.png)

![](/mdImg/smallProgram2.png)

## 小程序文件结构

### 基本的结构

```
- pages // 包含了所有页面
	- index // 页面文件夹
		- index.js    // 页面的脚本逻辑文件
		- index.wxml  // 页面模板文件
		- index.wxss  // 页面样式文件
		- index.json  // 页面配置文件
- utils // 普通的工具函数
- app.js   // 项目启动入口
- app.json // 全局的配置
- app.wxss // 全局样式
- project.config.json // 项目的配置文件
```

和web的页面结构比较：

|  文件  |  小程序  |     web      |
| :--: | :---: | :----------: |
|  模板  | .wxml |    .html     |
|  样式  | .wxss |     .css     |
|  脚本  |  .js  | .js/script标签 |

### 全局配置 app.json

**app.json**文件用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多tab等。

#### 常见的属性：

|   属性   |   类型   |  必填  | 描述        | 详细配置地址                                   |
| :----: | :----: | :--: | :-------- | ---------------------------------------- |
| pages  | String |  是   | 页面路径列表    | https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#pages |
| window | Object |  否   | 全局的窗口样式   | https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#window |
| tabBar | Object |  否   | 底部tab栏的配置 | https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#tabBar |

#### 小知识点：

- pages数组中的页面路径地址必须存在pages文件夹中
- pages数组的页面路径地址下标为0,也就是第一个路径在普通编译模式下会作为启动页面，但不建议使用更换顺序的方式修改启动页，可以通过新增或修改编译模式更改启动页

#### 示例：

```js
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs",
    "pages/demo/demo",
    "pages/login/login"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#FF5809",
    "navigationBarTitleText": "淘宝",
    "navigationBarTextStyle":"white"
  },
  "tabBar": {
    "selectedColor":"#0094FF",
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "首页",
        "iconPath": "/images/home.png", 
        "selectedIconPath": "/images/homeSelected.png" 
      },
      {
        "pagePath": "pages/demo/demo",
        "text": "分类",
        "iconPath": "/images/sale.png",
        "selectedIconPath": "/images/saleSelected.png"
      },
      {
        "pagePath": "pages/login/login",
        "text": "我",
        "iconPath": "/images/mine.png",
        "selectedIconPath": "/images/mineSelected.png"
      }
    ]
  }
}

```

![](/mdImg/微信小程序1.png)

## rpx尺寸单位

![](/mdImg/smallProgram3.png)

> rpx可以使原生根据屏幕进行自适应，小程序规定屏幕的宽度为750rpx，也就是100% = 750rpx; 50% = (750/2)rpx

![](/mdImg/微信小程序5.png)

### 使用方式

在开发小程序时建议使用750rpx像素的设计稿，这样设计稿的元素宽度是多少像素就直接设置为多少rpx

```
注意1rpx在某些屏幕上可能无效
```

![](/mdImg/微信小程序4.png)

## 样式导入

> 通过`@import`导入外部的样式

导入方法，参考weui-wxss提供的例子:

![](/mdImg/微信小程序2.png)

## 总结

1. wxss 不需要导入到 wxml ，在页面目录下创建即可生效
2. rpx 尺寸单位
3. @import 样式导入
4. app.wxss是全局样式,page中的wxss是页面的局部样式