---
title: 学习React Native:Mac环境搭建Nuclide安装与配置
date: 2016-04-06 14:10:18
tags: [React Native,ios]
categories: [iOS]
grammar_cjkRuby: true
---

Facebook开源了能够使用`JavaScript`开发iOS和Android原生应用的React Native。同时，Facebook还为React Native开发了一款基于跨平台文本编辑器Atom的开源IDE：Nuclide。

Nuclide的官网：http://nuclide.io
Nuclide的github：https://github.com/facebook/nuclide
Nuclide是Facebook开发的开发React Native的开发工具，基于Github的Atom开发,是基于Atom 的提供的一系列插件集。要使用 Nuclide ，首先需要先安装 Atom 。
官网下载地址：(Atom)[https://atom.io/]


**安装方法一：**
(1)安装完 Atom 后，打开 Settings 面板，并点击 Install 选项卡，然后在搜索框中键入 `nuclide-installer`

选择我们需要安装的插件，点击该插件旁边的蓝色 Install 按钮进行安装。

另一种方法是直接利用 Atom 的包管理器 apm 安装：
`$ apm install nuclide-installer`

**安装方法二：**
(1)还有一种方法，对Nuclide源代码编译安装, Nuclide项目官方地址:https://github.com/facebook/nuclide，我们知道该项目是facebook官方提供的
```powershell
git clone https://github.com/facebook/nuclide.git
cd nuclide
npm install
apm link
```

安装好了就可以高效的进行ReactNative的学习与开放了

###  插件推荐
- atom-beautify 代码格式化
- atomerminal 打开终端，pwd为当前文件所在路径
- docblockr 写注释的插件
- react：React 的语法补全和智能重排；
- save-session：让 Atom 记住上一次打开的会话；
- browser-plus：在 Atom 中内嵌一个浏览器窗口，方便页面调试（其实 Atom 本身就是一个浏览器）；
- react-snippets：React 的代码段；
- highlight-selected：高亮当前双击选中的标记；
- jshint：检查 JavaScript 的语法，支持 JSX （需要在插件设置中开启 Support Linting JSX）；
- emmet：用 emmet （Zen Coding）方式快速编写页面；