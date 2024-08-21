---
title: FFMpeg集成步骤
date: 2017-09-10 11:13:38
tags: FFMpeg
categories: [iOS]
grammar_cjkRuby: true
---

#### FFMpeg集成步骤

##### 第一步：下载[FFmpeg脚本地址](https://github.com/kewlbear/FFmpeg-iOS-build-script)

##### 第二步：下载完整的ffmpeg支持库。

终端进入脚本目录，执行下载脚本

```shell
cd /Users/xxx/FFmpeg-iOS-build-script-master
./build-ffmpeg.sh
```

下载完毕后会发现多了很多文件夹，其中FFmpeg-iOS文件是我们在项目中需要用到的，另外ffmpeg-3.x文件是全平台下载的编译文件（包含了TVOS、Mac OS、iOS等）。

上面下载这步非常耗时



所以推荐下面方式

当然你也可以直接下载编译好的库文件，不保证支持**bitcode**

You can download a binary for FFmpeg 3.3 release at <https://downloads.sourceforge.net/project/ffmpeg-ios/ffmpeg-ios-master.tar.bz2>



 FFmpeg-iOS是编译出来的库，里面是我们需要的.a 静态库，一共有7个。 进入.a文件目录终端输入

```
lipo -info libavcodec.a
//输出 libavcodec.a are: armv7 i386 x86_64 arm64
```

可以查看.a 包支持的架构，包括 armv7 armv7s i386 x86_64 arm64这几个架构。



##### 第三步：集成 iOS平台下的ffmpeg

将FFmpeg-iOS拖入工程

然后在`Taget`- `Build Settings` 中找到 `Search Paths` ，设置 `Header Search Pahts` 和 `Library Search Paths` 如下。不然会报 `include“libavformat/avformat.h” file not found ` 错误。

Header和Library中分别输入

```
1、$SRCROOT/FFmpeg-iOS/include
2、$(PROJECT_DIR)/FFmpeg-iOS/lib //一般拖入文件会默认生成
```



还有几个库文件
libz.dylib, libiconv.dylib ,libiconv2.4.0.dylib

https://cnbin.github.io/blog/2015/05/19/iospei-zhi-ffmpegkuang-jia/

https://github.com/qiangxinyu/XYFFmpeg

https://github.com/newtechsoft/RTSPPlayer