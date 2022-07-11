---
title: Mac  ffmpeg 使用
date: 2016-05-05 13:39:15
tags: 工具
categories: [工具]
grammar_cjkRuby: true
---

1、安装ffmpeg---`brew install ffmpeg`

如果想要添加`libfdk_aac`或`libvpxd`等库(这两个库是高度推荐安装的)，可以输入以下包含额外推荐选项的命令:

```shell
brew install ffmpeg --with-fdk-aac --with-ffplay --with-freetype --with-libass --with-libquvi --with-libvorbis --with-libvpx --with-opus --with-x265
```

ffmpeg需要的依赖包几乎都可以通过brew install安装：

```shell
brew install automake celt faac fdk-aac lame libass libtool libvorbis libvpx libvo-aacenc opencore-amr openjpeg opus sdl schroedinger shtool speex texi2html theora wget x264 xvid yasm
```

也可以通过一下网址下载

- [http://evermeet.cx/ffmpeg/](http://evermeet.cx/ffmpeg/)
- [http://ffmpegmac.net/](http://ffmpegmac.net/)

2、查看ffmpeg info---`brew info ffmpeg`

3、升级ffmpeg   `brew update && brew upgrade ffmpeg`





示例

1. 使用命令`ffmpeg -i http://...m3u8 -c copy out.mkv`将视频流下载并合并成 out.mkv。



相关应用

视频格式转换器 

VidConvert 是一款非常不错的视频格式转换器，在开始使用之前，其需要安装转换引擎——FFmpeg

More：

[FFMPEG视音频编解码零基础学习方法](http://blog.csdn.net/leixiaohua1020/article/details/15811977)

http://www.qiaogao.net/lessons/2014/08/02/ffmpeg/

[小教程 利用 iFFmpeg 下载网上 .m3u8 的视频](http://www.macx.cn/thread-2118809-1-1.html)

# [HLS-iOS视频播放服务架构深入探究（二）](http://yangchao0033.github.io/blog/2016/02/14/hls-2/)

[一键下载M3U8/HLS 并保存为TS文件](一键下载M3U8/HLS 并保存为TS文件)

https://github.com/Minlison/MLSDownloader