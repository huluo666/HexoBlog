---
title: iOS开发 Xcode高级调试
date: 2016-06-14 17:48:34
tags: iOS
grammar_cjkRuby: true
---



第三方库

[**PonyDebugger**](https://github.com/square/PonyDebugger)

通过Chrom浏览器可以监控网络，还可以查看Core Data对象，view的层级查看

PS：在从App Store上下载Xcode后，默认是不会安装Command Line Tools的，Command Line Tools是在Xcode中的一款工具，可以在命令行中运行C程序。

**验证是否安装：**

打开Xcode，创建一个新的项目，在OS X下面选择Application,如果右侧出现Command line tool图标，表示已经安装成功。

**安装方法：**

终端中输入以下命令：`xcode-select --install`  按回车，根据提示操作。

[调试利器-PonyDebugger](http://lucifer1988.github.io/blog/2014/03/03/diao-shi-li-qi-ponydebugger/)

[ponyDebugger安装失败](http://xiongzenghuidegithub.github.io/blog/2016/03/21/ponydebuggeran-zhuang-shi-bai/)

[Safari 前端开发调试 iOS 完美解决方案（iPhone/iTouch 等）](Safari 前端开发调试 iOS 完美解决方案（iPhone/iTouch 等）)



**Bug排查**

dSYM文件--位于打包时的xcarchive包内容中

- 将打包发布软件时的xcarchive文件拖入软件窗口内的任意位置(支持多个文件同时拖入，注意：文件名不要包含空格)



相关文章推荐

[久违的的LLDB篇一，让lldb提升你的效率](http://www.jianshu.com/p/f888db82fc27)

[LLdb篇2教你使用faceBook的chisel来提高调试效率](http://www.jianshu.com/p/b2371dd4443b)

[Chisel-LLDB命令插件，让调试更Easy](https://blog.cnbluebox.com/blog/2015/03/05/chisel/)

https://github.com/Flipboard/FLEX

https://github.com/coderyi/NetworkEye

https://github.com/KnuffApp/Knuff

https://github.com/ryanolsonk/LLDB-QuickLook

https://github.com/huang303513/Debug-Instruments