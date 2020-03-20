---
title: 前端测试3_Jest命令行工具的使用
tags: Jest
categories: Jest
date: 2020-03-19
---

**vscode ctri+shift+p**

![](/mdImg/test18.png)

<!--more-->

![](/mdImg/test19.png)

## 情形一：只有一个测试用例未通过，改正之后我们只想重新跑一次这唯一一个测试用例

![](/mdImg/test20.png)

## `Press f to run only failed tests`

> 按f 键，只测试以前失败的测试。执行npm run test 的时候，发现有一个测试失败了，这时我们只想测试这个失败的测试，可以按f了。

![](/mdImg/test21.png)

##  `Press o to only run tests related to changed files`

> 按o ，只会去测试和当前被改变文件相关的测试。但这时，你按o，发现报错了。为什么呢？因为让Jest 去测试改变文件中的测试，但Jest它自己并不知道哪个文件发生了变化，Jest本身，不具备比较文件改变的功能，那怎么办？需要借助git. 因为git 就是追踪文件变化的，只要把工作区和仓库区的代码一对比，就知道哪个文件发生变化了。因此需要把项目变成git 项目。在根目录下，先建.gitignore 文件，再执行git init， 把项目变成git 项目，否则会把node_modules 放到 git 仓库中。

![](/mdImg/test22.png)

然后进入o模式，只修改其中一个文件

![](/mdImg/test23.png)

#### 修改配置文件，默认进入o模式

```js
{
    "name": "day02",
    "version": "1.0.0",
    "description": "",
    "main": "math.js",
    "scripts": {
        "test": "jest --watch" // this
    },
    "author": "",
    "license": "ISC",
    "devDependencies": {
        "@babel/core": "^7.4.5",
        "@babel/preset-env": "^7.4.5",
        "jest": "^24.8.0"
    }
}
```

![](/mdImg/test24.png)

## `Press p to filter by a filename regex pattern.`

> 再看一下p, 按照文件名执行测试，我们提供一个文件名，它只会对该文件进行测试，可以使用正则表达式来匹配文件名。按p, 提示输入pattern, 再输入fetch, 它就会用fetch 去匹配所有的测试文件名，找到了fetchData.test.js 测试文件，然后它就执行了。如果找不到任何测 试文件，它什么测试都不会执行。

## `Press t to filter by a test name regex pattern.`

> t 则是匹配的test 名字，每一个test 都有一个描述，这个描述可以称之为test 的名字。提供一个test 的名字，它只跑这个test，用法和p 一样。

## `Press q to quit watch mode`

> 退出watch

## `Press Enter to trigger a test run`

> 就是跑一次单元测试，无论是在什么模式下，只要按enter，就会跑一次对应模式的测试