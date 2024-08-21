---
title: iOS内存泄漏
date: 2018-03-27 15:44:38
tags:
categorys:
---



#### Instrument — Leaks，Allocations，Analyze

**Analyze**-静态分析工具

Product->Analyze(`command+shift+B`)

可以找出代码潜在错误,如内存泄露,未使用函数和变量等,还可以检测出一些内存泄漏问题，比如一些比较明显的循环引用，CF库对象未release等相对简单的问题，通常是在进行其他方式检测之前就使用的方式，把一些简单的问题先发现并处理了。

主要分析以下四种问题

- 逻辑错误：访问空指针或为初始化的变量等；
- 内存管理错误：如内存泄漏等；
- 声明错误：从未使用过的变量
- Api调用错误：未包含使用库和框架

 Allocations工具是一个跟踪由应用程序分配的对象内存的工具。可以用来在疑似内存泄露的地方，通过反复操作，查看某些对象内存是否有被正常的释放，从而得知是否发生内存泄露。

**Instruments**-动态分析工具（product -> profile）

内存泄漏检测工具-Leaks

Instruments工具(`command+I`)-选择Leaks选项

选择Target，在右下角Display Setting面板的Call Tree，勾选Invert Call Tree和Hide System Libraries，方便接下来我们迅速查找有内存问题的代码段。



#### 第三方工具

相对于系统工具，方便快速多了

#### 一、MLeaksFinder

精准 iOS 内存泄露检测工具

<https://github.com/Tencent/MLeaksFinder>

直接拖入`MLeaksFinder`文件夹到工程即可

**MLeaksFinder 新特性** <https://wereadteam.github.io/2016/07/20/MLeaksFinder2/>

MLeaksFinder的原理：

使用runtime方法交换，它swizzle了NavigationController的Push和Pop相关方法来管理viewController和view的生命周期, 在你Pop掉viewController的时候, 会执行这么一段代码

```
__weak id weakSelf = self;
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(3 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        [weakSelf assertNotDealloc];
    });

```

3秒后执行 [weakSelf assertNotDealloc]; 如果这个时候view和viewController已经释放了, 那么weakSelf应该为nil, 所以将不会触发断言, 否则将会打印日志, 触发断言.

MLeaksFinder的原理还是很简单的, 它swizzle了NavigationController的Push和Pop相关方法来管理viewController和view的生命周期, 在你Pop掉viewController的时候, 会执行这么一段代码

```
__weak id weakSelf = self;
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(3 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        [weakSelf assertNotDealloc];
    });

```

3秒后执行 [weakSelf assertNotDealloc]; 如果这个时候view和viewController已经释放了, 那么weakSelf应该为nil, 所以将不会触发断言, 否则将会打印日志, 触发断言.

**OOMDetector**

https://github.com/Tencent/OOMDetector

同样来自腾讯，OOMDetector是一个iOS内存监控组件，应用此组件可以帮助你轻松实现OOM监控、大内存分配监控、内存泄漏检测等功能。

[【腾讯开源】iOS爆内存问题解决方案-OOMDetector组件](https://juejin.im/post/5a58f1a76fb9a01cab283392)



#### 二、PLeakSniffer

iOS内存泄漏自动检测工具

<https://github.com/music4kid/PLeakSniffer>

<http://mrpeak.cn/blog/leak/>



在`AppDelegate.m`加入

```objectivec
#if DEBUG
[[PLeakSniffer sharedInstance] installLeakSniffer];
[[PLeakSniffer sharedInstance] addIgnoreList:@[@"MySingletonController"]];//需要忽略的文件
[[PLeakSniffer sharedInstance]  alertLeaks];//使用弹出框提示
#endif
```

#### 三、使用Facebook开源检查工具FBRetainCycleDetector

使用的是MRC机制的文件，需要设置对应文件的Compiler Flags为`-fno-objc-arc`。

如何使用，使用`FBRetainCycleDetector`，需要将以上4个文件改成`-fno-objc-arc`

```objectivec
FBBlockStrongLayout
FBAssociationManager
FBBlockStrongRelationDetector
FBClassStrongLayoutHelpers
```











