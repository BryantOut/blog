---
title: P1_TypeScript介绍、安装及开发工具
tags: TypeScript
categories: TypeScript
date: 2020-02-17
---

[TS入门教程](https://ts.xcatliu.com/ )

# P1 TypeScript 介绍、安装及开发工具

# TypeScript 介绍

1. **TypeScript** 是由微软开发的一款开源的编程语言。
2. **Typescript** 是 **Javascript** 的超集，**遵循最新的 ES6、ES5 规范**。**Typescript** 扩展了 **Javascript** 的语法。
3. **TypeScript** 更像后端 **java**, **C#**这样的**面向对象语言**可以让 **js 开发大企业项目**。
4. 谷歌也在大力支持 **Typescript** 的推广，**谷歌的 angular2.x+ 就是基于 Typescript 语法**。
5. **最新的 Vue 、React 也可以集成 TypeScrupt** 。

<!--more-->

# TypeScript 安装 编译

TypeScript 的命令行工具安装方法如下 :

`npm install -g typescript `

以上命令会在全局环境下安装 `tsc` 命令，安装完成之后，我们就可以在任何地方执行 `tsc` 命令了。

编译一个 TypeScript 文件很简单

`tsc index.ts `

![](/mdImg/ts1.png)

我们约定使用 TypeScript 编写的文件以 `.ts` 为后缀，用 TypeScript 编写 React 时，以 `.tsx` 为后缀。 

# TypeScript 开发工具 Vscode 自动编译 .ts 文件

1. 创建 `tsconfig.json` 文件 `tsc --init` 生成配置文件

![](/mdImg/ts2.png)

2. 点击菜单 `任务-运行任务` 点击 tsc:监视`tsconfig.json`，然后就可以自动生成代码

> 无法加载文件 .ps1 ，因为在此系统中禁止执行脚本

以管理员身份运行powershell 

`set-executionpolicy remotesigned`

输入y即可 

`[Y] 是(Y)  [N] 否(N)  [S] 挂起(S)  [?] 帮助 (默认值为“Y”):`

# TypeScript 开发工具 HBulider 自动编译 .ts 文件

![](/mdImg/ts3.png)

# TypeScript 开发工具 HBuliderX 自动编译 .ts 文件

![](/mdImg/ts4.png)