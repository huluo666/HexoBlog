---
title: runtime之方法交换
date: 2016-03-09 14:20:58
tags: runtime,小书匠
description: runtime方法交换
---

利用runtime熟悉项目架构
团队项目比较多，有时需要接手别的项目，跳转流程是怎样的，需要修改的控制器叫什么名字。。？这些都不知道，肿么办。。。
下面利用Runtime方法交换原理，可以方便的打印你所进入的控制器，方便调试。
<!--more-->

## 原理简介：
方法交换，顾名思义，就是将两个方法的实现交换。例如：将A方法和B方法交换，调用A方法的时候，就会执行B方法中的代码，反之亦然。
 1.SEL – 表示该方法的名称又名选择器；
 2.types – 表示该方法参数的类型；
 3.IMP – 指向该方法的具体实现的函数指针，也就是就是实现方法。
参考：http://nshipster.com/method-swizzling/

## 新建Category文件
`UIViewController+SwizzleMethods.h`

```objectivec
//  UIViewController+SwizzleMethods.h
//  RuntimeLearn
//
//  Created by luo.h on 16/3/3.
//  Copyright © 2016年 appledev. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface UIViewController (SwizzleMethods)

@end
```

`UIViewController+SwizzleMethods.m`

```objectivec
//
//  UIViewController+SwizzleMethods.m
//  RuntimeLearn
//
//  Created by luo.h on 16/3/3.
//  Copyright © 2016年 appledev. All rights reserved.
//

#import "UIViewController+SwizzleMethods.h"
#import <objc/runtime.h>

@implementation UIViewController (Swizzle)

//load方法会在类第一次加载的时候被调用 相关：http://blog.leichunfeng.com/blog/2015/05/02/objectivec-plus-load-vs-plus-initialize/
//调用的时间比较靠前，适合在这个方法里做方法交换
//注意：不要在+initialize中交换
//+initialize是类第一次初始化时才会被调用，因为这个类有可能一直都没有使用到，因此这个类可能永远不会被调用。
+ (void)load{
    //方法交换应该被保证，在程序中只会执行一次
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
         //方法交换
        SwizzlingMethods([self class], @selector(viewWillAppear:), @selector(swiz_viewWillAppear:));
    });
}


/** 方法交换*/
void SwizzlingMethods(Class cls, SEL systemSel, SEL newSel)
{
    //两个方法的Method
    Method systemMethod = class_getInstanceMethod(cls, systemSel);
    Method newMethod = class_getInstanceMethod(cls, newSel);

    //首先动态添加方法，实现是被交换的方法，返回值表示添加成功还是失败
    BOOL isAddMethod = class_addMethod(cls,systemSel,
                                       method_getImplementation(newMethod),
                                       method_getTypeEncoding(newMethod));
    if(isAddMethod){
        //如果成功，说明类中不存在这个方法的实现
        //将被交换方法的实现替换到这个并不存在的实现
        class_replaceMethod(cls, newSel, method_getImplementation(systemMethod), method_getTypeEncoding(systemMethod));
    }else{
        //否则，交换两个方法的实现(改变两个方法的具体指针指向)
        method_exchangeImplementations(systemMethod, newMethod);
    }
}

- (void)swiz_viewWillAppear:(BOOL)animated{
    //看起来像是死循环，但是其实自己的实现已经被替换了
    //防止UINavigationController 或 UIInputWindowController调用次数变多
    if ([NSStringFromClass([self class]) isEqualToString:@"UINavigationController"] || [NSStringFromClass([self class]) isEqualToString:@"UIInputWindowController"]) {
        return;
    }
    [self swiz_viewWillAppear:animated];
    NSLog(@"✅VC->%@",[self class]);
}
@end

```






