---
title: shell截图命令：screencapture
date: 2017-02-04 17:32:47
tags: iOS
grammar_cjkRuby: true
---

使用方法：

```
screencapture -i test.png

```

执行后会调用系统默认的截图程序，也就是按`cmd-shift-4`出来的那个截图程序。截图完毕后，会保存到test.png中。

`-i`模式默认是自由模式，按一下空格键，可以在自由模式和窗口模式间切换。按下control键，截图就不会保存到文件中，而是保存到剪贴板中。

screencapture命令的其他选项：

```
-c         强制截图保存到剪贴板而不是文件中
-C         截图时保留光标（只在非交互模式下有效）
-d         display errors to the user graphically（不知道啥意思）
-i         交互模式截取屏幕。可以是选区或者是窗口。
             control key - 截图保存到剪贴板
             space key   - 在鼠标选区模式和窗口模式间切换
             escape key  - 退出截图
-m         只截取主显示器（-i模式下无效）
-M         截图完毕后，会打开邮件客户端，图片就躺在邮件正文中
-o         在窗口模式下，不截取窗口的阴影
-P         截图完毕后，在图片预览中打开
-s         只允许鼠标选择模式
-S         窗口模式下，截取屏幕而不是窗口
-t<format> 指定图片格式，模式是png。可选的有pdf, jpg, tiff等
-T<seconds> 延时截取，默认为5秒。
-w         只允许窗口截取模式
-W         开始交互截取模式，默认为窗口模式（只是默认模式与-i不同）
-x         不播放声效
-a         do not include windows attached to selected windows（不懂）
-r         不向图片中加入dpi信息
-l<windowid> 抓取指定windowid的窗口截图
-R<x,y,w,h> 抓取指定区域的截图
-B<bundleid> 截图输出会被bundleid指出的程序打开
```





cmd+shift+3：捕获整个屏幕

cmd+shift+4：捕获选择的区域

cmd+shift+4  再按space：捕获某个应用程序的窗口





如果在使用标准的截图快捷键的同时按下Control键，则不需要在桌面上生成文件，而是直接让图片进入剪贴板。

保存截图到桌面



按`Command＋shift＋4` 后 ,画一个抓取的区域，不要松开鼠标，接着

```
1. 按住空格可以移动这个区域
2. 按住 Shift后，将锁定X 或者 Y轴进行拖动
3. 按住 Option后 将按照区域圆心进行放大.
```

最后所有截图将直接显示在桌面上。



截图也可以在屏保的使用，操作如下

首先，进入“`系统偏好设置” -> “桌面于屏幕保护” -> “屏幕保护程序`”。选择你想截屏的屏幕保护，按住Command-Shift，然后点击“测试”按钮。

等屏幕保护开始运行后，不要松开`Command-Shift`键，再按照自己需求按3/4键。一张屏幕保护的截屏就完成了。

这个可以应用到很多地方，发挥你的扩展思维吧~

默认截图文件名是中文的，如何截图文件名改成英文命名，文件名由两部分构成：前缀和时间戳。

首先来修改前缀。打开终端（可以在Spotlight中输入“Terminal”并点选“应用程序”右边的“终端”），输入以下命令

```
defaults write com.apple.screencapture name Screenshot
killall SystemUIServer
```

将蓝色部分替换为任意所需的单词即可，例如“`Screenshot`”。要让前缀修改生效，需要重新启动系统。



如何弄掉阴影

```
defaults write com.apple.screencapture disable-shadow -bool true
killall SystemUIServer
```

恢复

```
defaults write com.apple.screencapture disable-shadow -bool false
killall SystemUIServer
```