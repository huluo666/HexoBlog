---
title: iOS 11 和 iPhone X 适配问题集锦
tags: iOS
categories: iOS
date: 2017-04-13 10:04:51
grammar_cjkRuby: true
---

[iOS 11 和 iPhone X 适配问题集锦](http://cdn2.jianshu.io/p/f2032e8fe56e)
http://wetest.qq.com/lab/view/337.html
http://www.jianshu.com/p/f5ee206c7df0
https://developer.apple.com/cn/ios/update-apps-for-iphone-x/
http://cdn2.jianshu.io/p/17d522b4153d
http://www.cocoachina.com/ios/20171016/20800.html
http://www.jianshu.com/p/352f101d6df1
http://www.cnblogs.com/pengsi/p/7765279.html
iPhone X屏幕分辨率=1125x2436
屏幕高度=(1125x2436)/@3x=375x812;


```
#define ScreenWidth [[UIScreen mainScreen] bounds].size.width
#define ScreenHeight [[UIScreen mainScreen] bounds].size.height
#define Is_Iphone (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPhone)
#define Is_Iphone_X (Is_Iphone && ScreenHeight == 812.0)
#define NaviHeight Is_Iphone_X ? 88 : 64
#define TabbarHeight Is_Iphone_X ? 83 : 49
#define BottomHeight Is_Iphone_X ? 34 : 0


// 判断是否是iPhone X
#define iPhoneX ([UIScreen instancesRespondToSelector:@selector(currentMode)] ? CGSizeEqualToSize(CGSizeMake(1125, 2436), [[UIScreen mainScreen] currentMode].size) : NO)
// 状态栏高度
#define STATUS_BAR_HEIGHT (iPhoneX ? 44.f : 20.f)
// 导航栏高度
#define NAVIGATION_BAR_HEIGHT (iPhoneX ? 88.f : 64.f)
// tabBar高度
#define TAB_BAR_HEIGHT (iPhoneX ? (49.f+34.f) : 49.f)
// home indicator
#define HOME_INDICATOR_HEIGHT (iPhoneX ? 34.f : 0.f)


// 状态栏(statusbar)
#define STATUSBAR_HEIGHT ([[UIApplication sharedApplication] statusBarFrame].size.height)


CGRect StatusRect = [[UIApplication sharedApplication] statusBarFrame];

//标题栏

CGRect NavRect = self.navigationController.navigationBar.frame;


例子：
[view mas_makeConstraints:^(MASConstraintMaker *make) {
      make.top.equalTo(self.view.mas_top).with.offset(NaviHeight);
      make.bottom.equalTo(self.view.mas_bottom).with.offset(-(BottomHeight));
      make.left.right.equalTo(self.view);
  }];
//有tabbar就用TabbarHeight，没有就用BottomHeight
```

