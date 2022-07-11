---
title: Mac 开发基础教程
date: 2016-04-25 18:24:12
tags: Mac
categories: [Mac]
grammar_cjkRuby: true
---

[OS X中如何设置NSView的背景颜色](http://www.jianshu.com/p/80dcee893b2d)

```objectivec
-(void)setBackground:(NSColor *)aColor;
{
    [self setWantsLayer:YES];
    [self.layer setBackgroundColor:aColor.CGColor];
    [self setNeedsDisplay:YES];//不加有时失效
}
```

[NSTableView的简单使用示例](http://www.jianshu.com/p/e9119da446d5)

[自定义NSTableCellView](http://blog.csdn.net/nsbeidou/article/details/45243069)

 [[Cocoa\]_[初级]_[NSTableView--数据操作和表格操作要注意的问题]](http://blog.csdn.net/moqj_123/article/details/42270137)

 [[Cocoa\]_[NSTableView]_[基本使用]](http://blog.csdn.net/yepeng2014/article/details/49026773)

 [[Cocoa\]_[NSTableView]_[添加复选框]](http://blog.csdn.net/yepeng2014/article/details/49154747)



视图切换

[osx switch between two nsviews](http://stackoverflow.com/questions/15645930/osx-switch-between-two-nsviews)



打包工具

http://www.pc6.com/mac/188843.html

http://www.jb51.net/softs/421224.html



