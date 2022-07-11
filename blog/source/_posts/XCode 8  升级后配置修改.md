---
title: Xcode插件开发
date: 2016-05-05 20:18:41
tags: iOS
categories: [iOS]
grammar_cjkRuby: true
---

https://onevcat.com/2013/02/xcode-plugin/

**插件位置**

在 Xcode 启动的时候，它会检查插件目录

`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins`

```shell
$ cd /Users/pconline/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins
$ open .
```

由于开发测试会生成N多插件，直接删除.xcplugin后缀名文件即可。

`defaults delete com.apple.dt.Xcode DVTPlugInManagerNonApplePlugIns-Xcode-7.2.1`

### 通过模板实现

 1）Xcode插件开发模板

可通过[Alcatraz](https://github.com/alcatraz/Alcatraz)直接安装

地址：[https://github.com/kattrali/Xcode-Plugin-Template](https://github.com/kattrali/Xcode-Plugin-Template)

http://gold.xitu.io/entry/5707426a2e958a0057b504a1



`+ (void) pluginDidLoad: (NSBundle*) plugin`

方法。 该方法会在 Xcode 加载插件的时候被调用，可以用来做一些初始化的操作。通常这个类是一个单例，并 Observe 了

每个xcode版本UUID不同，获取UUID方法

`defaults read /Applications/Xcode.app/Contents/Info DVTPlugInCompatibilityUUID`



[Xcode插件开发教程指南](http://www.jianshu.com/p/c50174f56d69?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)



[怎样创建一个xcode插件 第一部分/3部分](http://blog.csdn.net/yohunl/article/details/50816829)

 [怎样创建一个xcode插件 第2部分/3部分](http://blog.csdn.net/yohunl/article/details/51010690)



## Xcode7 插件制作入门

https://github.com/luisobo/Xcode-RuntimeHeaders



