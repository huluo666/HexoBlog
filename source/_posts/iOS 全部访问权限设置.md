---
title: iOS访问权限设置
tags:
categories:
date: 2016-03-30 15:17:51
grammar_cjkRuby: true
---

iOS 全部访问权限设置(不断更新)

**HTTPS**网络访问

https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW33

```
在Info.plist中添加 App Transport Security Settings 类型 Dictionary ;
并在App Transport Security Settings 下添加 Allow Arbitrary Loads 类型Boolean, 值设为 YES

<key>NSAppTransportSecurity</key>
<dict>
<key>NSAllowsArbitraryLoads</key>
<true/>
</dict>
```



```
升到iOS10之后，需要设置权限的有：

麦克风权限：Privacy - Microphone Usage Description 是否允许此App使用你的麦克风？
相机权限： Privacy - Camera Usage Description 是否允许此App使用你的相机？
相册权限： Privacy - Photo Library Usage Description 是否允许此App访问你的媒体资料库？
通讯录权限： Privacy - Contacts Usage Description 是否允许此App访问你的通讯录？
蓝牙权限：Privacy - Bluetooth Peripheral Usage Description 是否许允此App使用蓝牙？
语音转文字权限：Privacy - Speech Recognition Usage Description 是否允许此App使用语音识别？
日历权限：Privacy - Calendars Usage Description
定位权限：Privacy - Location When In Use Usage Description
定位权限: Privacy - Location Always Usage Description
位置权限：Privacy - Location Usage Description
媒体库权限：Privacy - Media Library Usage Description
健康分享权限：Privacy - Health Share Usage Description
健康更新权限：Privacy - Health Update Usage Description
运动使用权限：Privacy - Motion Usage Description
音乐权限：Privacy - Music Usage Description
提醒使用权限：Privacy - Reminders Usage Description
Siri使用权限：Privacy - Siri Usage Description
电视供应商使用权限：Privacy - TV Provider Usage Description
视频用户账号使用权限：Privacy - Video Subscriber Account Usage Description
```

plist信息

```plist
<key>NSAppleMusicUsageDescription</key>
    <string>App需要您的同意,才能访问媒体资料库</string>
    <key>NSBluetoothPeripheralUsageDescription</key>
    <string>App需要您的同意,才能访问蓝牙</string>
    <key>NSCalendarsUsageDescription</key>
    <string>App需要您的同意,才能访问日历</string>
    <key>NSCameraUsageDescription</key>
    <string>App需要您的同意,才能访问相机</string>
    <key>NSHealthShareUsageDescription</key>
    <string>App需要您的同意,才能访问健康分享</string>
    <key>NSHealthUpdateUsageDescription</key>
    <string>App需要您的同意,才能访问健康更新 </string>
    <key>NSLocationAlwaysUsageDescription</key>
    <string>App需要您的同意,才能始终访问位置</string>
    <key>NSLocationUsageDescription</key>
    <string>App需要您的同意,才能访问位置</string>
    <key>NSLocationWhenInUseUsageDescription</key>
    <string>App需要您的同意,才能在使用期间访问位置</string>
    <key>NSMotionUsageDescription</key>
    <string>App需要您的同意,才能访问运动与健身</string>
    <key>NSMicrophoneUsageDescription</key>
    <string>App需要您的同意,才能访问麦克风</string>
    <key>NSPhotoLibraryUsageDescription</key>
    <string>App需要您的同意,才能访问相册</string>
    <key>NSCameraUsageDescription</key>
    <string>App需要您的同意,才能访问相机</string>
    <key>NSRemindersUsageDescription</key>
    <string>App需要您的同意,才能访问提醒事项</string>
```

