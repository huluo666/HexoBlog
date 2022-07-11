---
title: iOS 代码优化，持续更新
date: 2016-04-18 10:36:27
tags: iOS
categories: iOS
grammar_cjkRuby: true
---

使用纯代码创建一个视图对象：

```objectivec
UIView *subView = [[UIView alloc] initWithFrame:[UIScreen mainScreen].bounds];
subView.backgroundColor = [UIColor whiteColor];
[self.view addSubview:subView];
```

这段代码可以转换成另外一种等价写法：

```objectivec
UIView *subView = ({
    UIView *view = [[UIView alloc] initWithFrame:[UIScreen mainScreen].bounds];
    view.backgroundColor = [UIColor whiteColor];
    [self.view addSubview:view];
    view;
});
```



使用字面量语言

```objectivec
NSArray *arrayA = [NSArray arrayWithObjects:object1, object2, object3, nil];
NSArray *arrayB = @[object1, object2, object3];//推荐
```

使用字面量语法的缺点：使用字面量创建都是不可变对象，如果想创建可变对象需要复制一份：

```objectivec
NSMutableArray *mutableArray = [@[@1, @2, @3] mutableCopy];
```



使用类型常量替换#define预处理指令

```swift
#define ANIMATION_DURATION 1.0
static const NSTimeInterval kAnimationDuration = 1.0;//推荐 命名规则：不被外部访问时 k+变量名
```



代码结构

```swift
#pragma mark - life cycle
// Methods...
#pragma mark - UITableViewDataSource
// Methods...
#pragma mark - CustomDelegate
// Methods...
#pragma mark - event response
// Methods...
#pragma mark - private methods
// Methods...
#pragma mark - getters and setters
// Methods...
```



获取当前导航控制器

```objectivec
#pragma mark Implement @protocol PCNavigationDelegate
- (UINavigationController *)navigationController
{
    UITabBarController *tabBar = (UITabBarController *)self.window.rootViewController;
    return (UINavigationController *)tabBar.selectedViewController;
}
```



[Effective-objectivec-读书笔记-Item-4-如何正确定义常量](http://tutuge.me/2015/03/11/Effective-objectivec-%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0-Item-4-%E5%A6%82%E4%BD%95%E6%AD%A3%E7%A1%AE%E5%AE%9A%E4%B9%89%E5%B8%B8%E9%87%8F/)

http://www.dewen.net.cn/q/5522/C+++%E5%90%84%E7%A7%8D%E5%85%A8%E5%B1%80%E5%B8%B8%E9%87%8F%E7%9A%84%E5%A3%B0%E6%98%8E%E6%96%B9%E5%BC%8F%E7%9A%84%E4%BC%98%E7%BC%BA%E7%82%B9%EF%BC%9F