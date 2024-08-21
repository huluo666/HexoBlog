---
title: iOS版本新特性(持续更新)
date: 2018-03-22 08:58:56
tags:
categorys:
---

iOS7~最新版SDK 版本新特性

https://onevcat.com

[开发者所需要知道的iOS7 SDK新特性](https://onevcat.com/2013/06/developer-should-know-about-ios7/)
[开发者所需要知道的 iOS8 SDK 新特性](https://onevcat.com/2014/07/developer-should-know-about-ios8/)
[开发者所需要知道的 iOS 9 SDK 新特性](https://onevcat.com/2015/06/ios9-sdk/)
[开发者所需要知道的 iOS 10 SDK 新特性](https://onevcat.com/2016/06/ios-10-sdk/)



**iOS 10**
SiriKit：仅限于几类应用（语音和视频通话、发送消息、发送或接收付款、搜索照片、约车、管理健身）
全新框架UserNotification.framework：统一了LocalNotification和PushNotification的处理方式
iMessage App
Xcode 8：View Debugging和Memory Debugging
Swift 3
NSAllowsArbitraryLoadsInWebContent
新的openURL:options:completionHandler:
方法替代openURL:

UICollectionViewDataSourcePrefetching

IDFA的变化：用户关闭广告追踪，返回00000000-0000-0000-0000-000000000000



**iOS 9**
iPad设备多任务：分屏、画中画等
watchOS 2
UI Test
Swift 2
App Thinning: asset Catalog，App会自行选择下载适合用户设备的一套资源
Bitcode：后续编译器更新时，代码会由Appstore自动编译优化
个人开发者可以将APP部署到自己的真机上调试了
3D Touch
内容搜索：spotlight、markup web content、NSUserActivity

ATS：从iOS 9开始默认开启对HTTPS的支持
Contacts.framework和ContactsUI.framework
NSURLConnection正式deprecated，替换为NSURLSession



**iOS 8**
App Extensions
可以使用Touch ID
验证身份
HealthKit
HomeKit
Handoff
Size Class
CoreLocation：引入了室内定位和重大位置改变服务
UIAlertController代替UIAlertView和UIActionSheet
注册Notification的方式发生变化
UIPresentationController
引入WKWebView



**iOS 7**
UI重新设计
TextKit
64位支持
新的后台任务
UIActivityViewController
边缘侧滑手势
CoreLocation引入区域监控（iBeacon）