---
title: Mac开发NSTask使用
date: 2016-05-04 15:13:38
tags: Mac
categories: [Mac]
grammar_cjkRuby: true
---

1、NSTask简介

NSTask是MAC OS X用来执行系统命令的一个类库。在Cocoa编程中遇到需要运行一些脚本或者Shell命令时，NSTask可能是一个不错的选择。使用NSTask类，你的程序可以执行另一个程序并获取程序运行的结果。

1、NSTask创建的是一个独立运行的进程，不会与主程序共享存储空间。

2、一个NSTask对象只能运行一次。之后试图运行任务会产生一个错误。



1、NSTask创建的是一个独立运行的进程，不会与主程序共享存储空间。
2、一个NSTask对象只能运行一次。之后试图运行任务会产生一个错误。
3、NSTask 在Swift 中与objectivec中的不同

	•	objectivec中, 是NSTask类
	•	Swift 中, 是Process类
#### 2. NSTask 在Swift 中与objectivec中的不同

- objectivec中, 是NSTask类
- Swift 中, 是Process类



#### 4. NSTask 相关权限

如果使用NSTask访问网络，或存取数据，需要开启以下权限

1、开启网络访问权限

2、开启了用户选择文件的读写权限



#### 7. 后语

关于NSTask的使用并不十分复杂，但如果想实现强大的需求,最好有一些必备的Shell编程知识，另外值得注意就是沙盒权限问题,文中的一下疑问或者意见,大家可以写在评论区进行讨论，最后希望大家周末愉快~