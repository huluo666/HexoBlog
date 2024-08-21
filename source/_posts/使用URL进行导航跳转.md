---
title: 使用URL进行导航跳转
date: 2016-04-19 13:46:28
tags: iOS
grammar_cjkRuby: true
---

适用于推送或网页跳转原生界面，目前公司的导航控制器跳转方式使用的URL方式跳转，这种方式也在很多App上使用。

```wiki
优点：1.后台配置URL，App集中处理所有URL，方便管理

	    2.一行代码即可进行跳转


缺点：1.使用的是字符串，系统无自动纠错能力，可能出错

	    2.传统方式可以点击控制器快速进入指定控制器，而URL不能，调试比较不便

```



结论：推送，网页协议等使用URL进行导航控制器跳转，原生跳转使用传统方式跳转。发挥URL灵活性和传统方式的DeBug时方便特点。



其实无论是传统方式，或是URL方式，亦或是其它方式进行调整，都要满足2个条件。

```wiki
1.得到需要跳转的控制器实例
2.获取当前导航控制器
```



写法有多种，关键点在于利用runtime实例对象，获取导航控制器跳转

如使用类别

```objectivec
//id viewController = [[class alloc] init];
- (void)pushViewControllerURL:(NSString *)aClass
{
    NSLog(@"topViewController=%@",[self.navigationController topViewController]);

    Class ClassName = NSClassFromString(aClass);
    UIViewController * viewController = [[ClassName alloc] initWithNibName:NSStringFromClass(ClassName) bundle:nil];

    //防止连续push多次
    if ([[self.navigationController topViewController] isKindOfClass:[viewController class]]) {
        return;
    }
    [self.navigationController pushViewController:viewController animated:YES];
}
```



以上只是简单示例，实际开发种有很多种情况需要考虑，可以参照以下Demo，

随便一搜，发现相关例子一大堆，所以可参照Demo设计出适合自己项目的跳转模式。



https://github.com/GeniusBrother/HZURLManager

https://github.com/gaosboy/urlmanager

https://github.com/Huohua/HHRouter

https://github.com/uxyheaven/XYRouter

https://github.com/liaojinxing/LJURLRouter

https://github.com/gonefish/GQURLDispatcher

https://github.com/SwiftLOL/AppNavigator



