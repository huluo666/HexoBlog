---
title: 利用runtime进行夜间模式图片加载
date: 2016-05-11 16:35:26
tags: iOS
grammar_cjkRuby: true
---

项目之前利用自定义类方法实现夜间模式图片的加载，但是用的自定义方法显然没有系统方法看起来舒服，代码移植性好，更重要的是不能使用KSImageNamed插件提示图片，相比之前各种前后缀，分辨率等判断，利用`runtime`看起来简洁多了,使用方法与系统一样。

说明：对于大部分图片是不分白天，夜间模式的，所有首先查找正常图片，如果没有找到，判断App为哪种模式进行加载。白天模式-图片加上后缀`_day`，夜间`_night`



```objectivec
#import "UIImage+imageNamed.h"
#import <objc/runtime.h>

@implementation UIImage (imageNamed)

+ (void)load{
    //方法交换应该被保证，在程序中只会执行一次
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        //方法交换
        Method m1 = class_getClassMethod([UIImage class], @selector(imageNamed:));
        Method m2 =  class_getClassMethod([UIImage class], @selector(HJ_imageNamed:));
        method_exchangeImplementations(m1, m2);
    });
}


//自定义加载方法
+(UIImage *)HJ_imageNamed:(NSString *)imageName{
    UIImage *result = nil;
    result = [UIImage HJ_imageNamed:imageName];
    BOOL isNight=YES;
    //2、白天、夜间模式
    if (!result) {
        if (isNight) {
            imageName = [imageName stringByAppendingString:@"_night"];
        } else {
            imageName = [imageName stringByAppendingString:@"_day"];
        }
        result = [UIImage HJ_imageNamed:imageName];
    }
    return result;
}

@end
```