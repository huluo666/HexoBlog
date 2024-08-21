---
title: EasyRTC
date: 2017-10-10 18:24:12
tags: 直播
categories: 直播
grammar_cjkRuby: true
---



EasyRTC是什么

EasyRTC是WebRTC标准的一个实现。



它包括：

- 服务器端，nodejs实现的

- Web端javascript api，相当于是对WebRTC API的封装和简化

- iOS和Android本地API，这个应该是付费版本才有

  ​

他们开源了服务器端和Web端的javascript api，见[GitHub](https://github.com/priologic/easyrtc)。





EasyRTC使用

1、安装nodejs，安装npm （没安装自行谷歌）

2、安装运行easyrtc需要的js包（js框架），package.json 中的依赖库

```shell
git clone https://github.com/priologic/easyrtc.git
cd   easyrtc/server_example 目录
npm install  //安装package.json中的依赖库
```

3、启动easyrtc server

```
 npm start
或
 node  server.js
//端口预设在 port 8080 LISTEN
```



4、浏览器中测试

打开http://localhost:8080 







2、引入必要的依赖库

```
AudioToolbox.framework
VideoToolbox.framework
QuartzCore.framework
OpenGLES.framework
CoreGraphics.framework
CoreVideo.framework
CoreMedia.framework
CoreAudio.framework
AVFoundation.framework
GLKit.framework
CFNetwork.framework
Security.framework
libsqlite3.tbd
libicucore.tbd
libc.tbd
libc++.dylib 
libstdc++.6.0.9.tbd
```





错误解决

```
Undefined symbols for architecture x86_64:
  "_OBJC_CLASS_$_RTCEAGLVideoView", referenced from:
      objc-class-ref in VideoChatViewController.o
  "_OBJC_CLASS_$_RTCSessionDescription", referenced from:
```

重新导入对于库文件

- framework文件没有导入
- 静态库编译时往往需要一些库的支持，查看你是否有没有导入的库文件
  同样是在`Build Phases`里的`Link Binary With Libraries`中添加相应的framework。



1、注意真机关闭`bitcode`

2、注意添加相关权限

[iOS10 权限崩溃问题](http://www.jianshu.com/p/83db0b4f0bfe)

```
<key>NSCameraUsageDescription</key>    
<string>cameraDesciption</string>
<key>NSContactsUsageDescription</key>
<string>contactsDesciption</string>
<key>NSMicrophoneUsageDescription</key>
<string>microphoneDesciption</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>photoLibraryDesciption</string>
```



[WebRTC各种资料集合](http://www.zaomaiwang.com/html/9125204.html)

[Webrtc实践](http://www.pffair.com/blog/2016/06/29/webrtcshi-jian/)

[WebRTC iOS平台的基本实现](http://www.cnblogs.com/fulianga/p/5869208.html)

[iOS下音视频通信-基于WebRTC](http://www.jianshu.com/p/c49da1d93df4)



[WebRTC iOS&OSX 库的编译](http://www.enkichen.com/2017/05/12/webrtc-ios-build/)

[iOS音视频开源框架WebRTC入门-编译(前序-授人鱼不如授人以渔)](http://www.kadia-china.com/?p/435753014d47)

[WebRTC入门：iOS工程](http://www.jianshu.com/p/994f9f6c9874)



[WebRTC学习笔记_Demo收集](http://www.cnblogs.com/hrhguanli/p/3781385.html)

https://github.com/AnyRTC/anyRTC-RTMP-OpenSource

https://github.com/qq3200341/FLWebRTCDemo

https://github.com/Haley-Wong/WebRTC_iOS

https://github.com/hiroeorz/PeerObjectiveC



http://blog.csdn.net/csdnhaoren13/article/details/51023814

[WebRTC有前途吗？](https://www.zhihu.com/question/22301898)

服务器

https://naurudao.blogspot.hk/2016/07/webrtc.html

