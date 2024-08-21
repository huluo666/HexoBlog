---
title: 图片无损压缩工具
date: 2016-12-28 15:24:12
tags: iOS
grammar_cjkRuby: true
---

#### 一、Mac shell命令

单张/批量修改图片尺寸

```
sips -Z 640 *.jpg //批量 604*640
sips -z 768 1024 example.png//单张 760*1024
```

More From：http://www.ainotenshi.org/818/resizing-images-using-the-command-line

图片格式转换

单张图片

```shell
$ sips -s format [image type] [file name] --out [output file]
$ sips -s format png test.jpg --out test.png
```



批量转换,转换到当前目录中的一个新的Converted子文件夹

```shell
$ for i in [filename]; do sips -s format [image type] $i --out [destination]/$i.[extension];done
$ for i in *.jpeg; do sips -s format png $i --out Converted/$i.png;done
```



Mac应用

https://imageoptim.com/mac

 https://medium.com/@yeong.crypto/optimizing-converting-images-8d10ae559586

https://www.cnblogs.com/lhb25/p/12-best-image-compression-tools.html

[图片压缩不求人7款超实用的压缩神器推荐](https://www.uisdc.com/7-practical-compression-tool)

[也谈图片压缩]( http://zhengxiaoyong.me/2017/04/23/%E4%B9%9F%E8%B0%88%E5%9B%BE%E7%89%87%E5%8E%8B%E7%BC%A9/)





[在Mac上使用Google图片压缩工具Guetzli](https://www.jianshu.com/p/565e944bb594)

 





#### 2、在线压缩

TinyPNG https://tinypng.com/ 【荐】

https://goimg.io/

**图好快** http://www.tuhaokuai.com/

https://www.picdiet.com/

https://compressionbear.com/

http://bigjpg.com/

http://isparta.github.io/index.html



**GIF压缩**

soogif http://www.soogif.com/compress

http://www.piggif.com/

https://ezgif.com/optimize

http://gifcompressor.com/zh/

https://shortpixel.com/online-image-compression

http://nullice.com/limitPNG/







https://github.com/leecade/imagine

https://imageoptim.com/

http://imgonline.com.ua/eng/compress-image.php

[Image Compression tools via command line](http://stackoverflow.com/questions/19153122/image-compression-tools-via-command-line)

[jpegoptim – the compression tool extraordinaire](http://danielhordern.com/2014/os-x-compress-jpeg-images-via-the-command-line/)

http://www.creativebloq.com/design/image-compression-tools-1132865

[Efficient Image Resizing With ImageMagick](https://www.smashingmagazine.com/2015/06/efficient-image-resizing-with-imagemagick/)

[图像压缩JPEG工具对比](http://www.jackenhack.com/image-compression-tools-web/)

[图片无损压缩工具都有哪些？](https://www.zhihu.com/question/19779256/answer/226924316)【知乎】

[8 款免费的图片压缩服务 & 工具推荐](https://zhuanlan.zhihu.com/p/29803076)

开源工具

https://github.com/saitjr/STTinyPNG-Python



https://juejin.im/entry/587f14378d6d810058a18e1f