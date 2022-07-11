---
title: iOS 多线程
date: 2016-05-27 15:22:41
tags: iOS
categories: iOS
grammar_cjkRuby: true
---

### 一、iOS的三种多线程技术　　

1.NSThread 每个NSThread对象对应一个线程，量级较轻（真正的多线程）

2.以下两点是苹果专门开发的“并发”技术，使得程序员可以不再去关心线程的具体使用问题

- NSOperation/NSOperationQueue 面向对象的线程技术


- GCD —— Grand Central Dispatch（派发） 是基于C语言的框架，可以充分利用多核，是苹果推荐使用的多线程技术



### 二、三种多线程技术的对比　　

•NSThread:

–优点：NSThread 比其他两个轻量级，使用简单

–缺点：需要自己管理线程的生命周期、线程同步、加锁、睡眠以及唤醒等。线程同步对数据的加锁会有一定的系统开销

 

•NSOperation：

–不需要关心线程管理，数据同步的事情，可以把精力放在自己需要执行的操作上

–NSOperation是面向对象的

 

•GCD：

–Grand Central Dispatch是由苹果开发的一个多核编程的解决方案。iOS4.0+才能使用，是替代NSThread， NSOperation的高效和强大的技术

–GCD是基于C语言的

![总结](https://o8ouygf5v.qnssl.com/Tables/ralationship.jpg)

[谈iOS多线程(NSThread、NSOperation、GCD)编程](http://www.jianshu.com/p/6e6f4e005a0b)

http://www.iosxxx.com/blog/2016-06-02-GCD%E9%82%A3%E4%BA%9B%E4%BA%8B.html