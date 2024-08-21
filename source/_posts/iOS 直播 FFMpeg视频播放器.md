---
title: FFMpeg视频播放器
date: 2017-09-10 11:13:38
tags: FFMpeg
categories: [iOS]
grammar_cjkRuby: true
---



git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-ios(备注:注意这个地址,不要自己去github上复制)



```shell
cd ijkplayer-ios
git checkout -B latest k0.4.5.1
./init-ios.sh
cd ios
./compile-ffmpeg.sh clean
./compile-ffmpeg.sh all
```

OK,至此就编译完成了

编译完是这个样子



https://guchunli.github.io/2017/06/29/iOS%E7%9B%B4%E6%92%AD%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/

[在Mac上自己搭建直播服务器](http://www.jianshu.com/p/ec0e9bc6591d)

[Mac上搭建直播服务器Nginx+rtmp](http://www.cnblogs.com/jys509/p/5649066.html)

[Mac本地推流直播服务器(nginx、rtmp服务器+ffmpeg推流)](http://hnxyzhw.xyz/2017/02/Mac%E6%9C%AC%E5%9C%B0%E6%8E%A8%E6%B5%81%E7%9B%B4%E6%92%AD%E6%9C%8D%E5%8A%A1%E5%99%A8(nginx-rtmp%E6%9C%8D%E5%8A%A1%E5%99%A8+ffmpeg%E6%8E%A8%E6%B5%81)/)



播放器下载

https://www.videolan.org/vlc/download-macosx.zh.html



[视频直播之推流](http://www.jianshu.com/p/e1f3ceba7bc4)

[直播解决方案整理](http://maxwellpro.cn/2017/02/09/%E7%9B%B4%E6%92%AD%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E6%95%B4%E7%90%86/)

iOS 直播 —— 推流



[最全的连麦直播技术点整理-AnyRTC](http://www.jianshu.com/p/0dfc105e281f)



[iOS中集成ijkplayer视频直播框架](http://www.jianshu.com/p/1f06b27b3ac0)



https://www.zybuluo.com/qvbicfhdx/note/126161



[做一款仿映客的直播App？看我就够了](http://www.jianshu.com/p/5b1341e97757)



https://aotu.io/notes/2016/10/09/HTML5-SopCast/index.html