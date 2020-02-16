---
title: vue项目中插件总结
tags: 插件
categories: 插件
date: 2018-9-11
---

# vue-quill-editor

[官方文档](https://github.com/surmon-china/vue-quill-editor)

![](/mdImg/vue-editor.png)

## 安装

```node
npm install vue-quill-editor --save
```

<!--more-->

## 在脚手架文件中的main.js中引入

```js
import Vue from 'vue'
import VueQuillEditor from 'vue-quill-editor'

// require styles
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'

Vue.use(VueQuillEditor, /* { default global options } */)
```

## 在组件中引用

```js
<el-tab-pane label="商品内容" name="5">
  <quill-editor v-model="addForm.goods_introduce" ref="myQuillEditor" :options="editorOption" 	@blur="onEditorBlur($event)" @focus="onEditorFocus($event)" @ready="onEditorReady($event)">
  </quill-editor>
</el-tab-pane>
```

# vue-echarts

[5分钟上手写ECharts的第一个图表](http://echarts.baidu.com/echarts2/doc/start.html)

## 安装

```node
npm install vue-echarts
```

## 在脚手架文件中的main.js中引入

```js
import ECharts from 'vue-echarts'
Vue.component('chart', ECharts)
```

## 在组件中引用

```js
<template>
  <div class='reports'>
    <chart :options='option'></chart>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        option: {
          title: {
            text: '折线图堆叠'
          },
          tooltip: {
            trigger: 'axis'
          },
          legend: {
            data: ['邮件营销', '联盟广告', '视频广告', '直接访问', '搜索引擎']
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
          },
          toolbox: {
            feature: {
              saveAsImage: {}
            }
          },
          xAxis: {
            type: 'category',
            boundaryGap: false,
            data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
          },
          yAxis: {
            type: 'value'
          },
          series: [
            {
              name: '邮件营销',
              type: 'line',
              stack: '总量',
              data: [120, 132, 101, 134, 90, 230, 210]
            },
            {
              name: '联盟广告',
              type: 'line',
              stack: '总量',
              data: [220, 182, 191, 234, 290, 330, 310]
            },
            {
              name: '视频广告',
              type: 'line',
              stack: '总量',
              data: [150, 232, 201, 154, 190, 330, 410]
            },
            {
              name: '直接访问',
              type: 'line',
              stack: '总量',
              data: [320, 332, 301, 334, 390, 330, 320]
            },
            {
              name: '搜索引擎',
              type: 'line',
              stack: '总量',
              data: [820, 932, 901, 934, 1290, 1330, 1320]
            }
          ]
        }
      };
    }
  };
</script>
```

# vue-amap

[官方文档](https://elemefe.github.io/vue-amap/#/)

![](/mdImg/vue-amap.png)

## 安装

```node
npm install -S vue-amap
```

## 在脚手架文件中的main.js中引入

```js
// 引入vue-amap
import VueAMap from 'vue-amap';
Vue.use(VueAMap);

// 初始化vue-amap
VueAMap.initAMapApiLoader({
  // 高德的key
  key: '6a204f2b675f32f8849ec4b6b7c21e5c',
  // 插件集合
  plugin: ['AMap.Autocomplete', 'AMap.PlaceSearch', 'AMap.Scale', 'AMap.OverView', 'AMap.ToolBar', 'AMap.MapType', 'AMap.PolyEditor', 'AMap.CircleEditor'],
  // 高德 sdk 版本，默认为 1.4.4
  v: '1.4.4'
});
```

## 在组件中引用

```js
<template>
  <div id="app">
    <div class="amap-wrapper">
      <el-amap class="amap-box" :vid="'amap-vue'"></el-amap>
    </div>
  </div>
</template>

<script>
export default {
}
</script>

<style>
.amap-wrapper {
  width: 500px;
  height: 500px;
}
</style>
```

