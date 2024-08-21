---
title: App内跳转至系统设置界面
date: 2016-04-11 16:34:52
tags: iOS
categories: iOS
grammar_cjkRuby: true
---

**直接跳转到该APP设置页面（WIFI,Location,photo...）**

```
if(&UIApplicationOpenSettingsURLString != nil)
{
	[[UIApplication sharedApplication] openURL:[NSURL URLWithString:UIApplicationOpenSettingsURLString]];
}
```

```
//定位服务设置界面
NSURL *url = [NSURL URLWithString:@"prefs:root=LOCATION_SERVICES"];
if ([[UIApplication sharedApplication] canOpenURL:url])
{
    [[UIApplication sharedApplication] openURL:url];
}
```

其它更多参考

https://gist.github.com/phynet/471089a51b8f940f0fb4

[iOS开发-跳转系统设置界面、App之间跳转、跳转AppStore](https://github.com/zilaiyedaren/zilaiyedaren.github.io/blob/b8211461a39bd0f1fc4c20252b0aca765fd8e6bb/_posts/2015-10-14-iOS%E5%BC%80%E5%8F%91-%E8%B7%B3%E8%BD%AC%E7%B3%BB%E7%BB%9F%E8%AE%BE%E7%BD%AE%E7%95%8C%E9%9D%A2%E3%80%81App%E4%B9%8B%E9%97%B4%E8%B7%B3%E8%BD%AC%E3%80%81%E8%B7%B3%E8%BD%ACAppStore.md)



PS：在iOS9带来的更新中，有一项关于`URL Scheme`的变化，具体内容是：在iOS9的SDK中，若要通过`URL Scheme`访问其他APP，则需要事先将该URL加入程序的白名单中。

**具体原因及解决方案查阅：**

[Querying URL Schemes with canOpenURL](http://useyourloaf.com/blog/querying-url-schemes-with-canopenurl/)

[iOS9URLScheme适配_引入白名单概念](https://github.com/ChenYilong/iOS9AdaptationTips/tree/master/Demo3_iOS9URLScheme%E9%80%82%E9%85%8D_%E5%BC%95%E5%85%A5%E7%99%BD%E5%90%8D%E5%8D%95%E6%A6%82%E5%BF%B5)

