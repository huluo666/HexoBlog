---
title: iOS 高效率编程工具篇
tags: [编程规范,代码优化]
date: 2016-03-30 18:15:10
categories: [iOS]
---

古语有云，“工欲善其事，必先利其器” ，合理的使用工具能让开发效率大大提升，达到事半功倍的效果。

### 第三方开源类库管理
CocoaPods--快速集成文第三方开源类库
使用教程：[CocoaPods安装和使用教程](http://code4app.com/article/cocoapods-install-usage)

### Xcode 插件
利用插件提高代码质量和工作效率

建议使用[Alcatraz](https://github.com/alcatraz/Alcatraz)来管理Xcode插件 快捷键`Shift+Command+9`

Alcatraz安装
```objectivec
curl -fsSL https://raw.github.com/alcatraz/Alcatraz/master/Scripts/install.sh | sh
```

Alcatraz卸载：		    
```objectivec
rm -rf ~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/Alcatraz.xcplugin

rm -rf ~/Library/Application Support/Alcatraz/
```

InjectionPlugin （`推荐`）   
Xcode高端必备插件，可以实时的修改代码,而不需要重新编译运行到模拟器中，成吨提高开发效率。

**代码格式化与注释**
1.XAlign   `Shift+Command+X`
2.ClangFormat

**注释辅助插件**
XToDo         快捷键标记，和统一查看(TODO，FIXME,???和!!!注释，还为你提供了一个查看列表。)
VVDocumenter  规范注释生成器

**JSON数据一键生成模型文件** 
1.ESJsonFormat
2.XWJsonToCode

**控制台的输出中文**
Debug栏打印时自动把Unicode转化成汉字
1.DXXcodeConsoleUnicodePlugin  
2.FKConsole

**import头文件整理**
项目中import了很多类，经过不停的修改后可能已经不需要导入这个类，可是我们没有删除#import，这样会导致我们编译速度变慢
FUI                 清除没有引用的头文件
CleanHeaders        清除没有引用的头文件
SortXcodeSelection  整理头文件

**XIB 辅助插件**
DefaultMarginDisabler  为XIB设置默认约束
RRConstraintsPlugin    使用自动布局的辅助插件.

FuzzyAutocompletePlugin 代码自动补全
XcodeBoost  提取方法声明，添加基于命令行的代码操作（剪切/复制/粘贴/重复/删除行），以及持续高亮等

DCLazyInstantiate 自动生成懒加载代码`Shift + Cmd + -`

ZLGotoSandboxPlugin 快速进入App沙盒目录
看沙盒的插件，当运行模拟器的时候，`Shift + Common + w` 直接查看当前模拟器下程序的沙盒。

CocoaPods    图像界面管理CocoaPods
FixCode      开发证书管理
KSImageNamed 提供了图像名称填写的自动化
KSHObjcUML   显示类的依赖关系导图
Reveal-In-GitHub 查看项目中GitHub的代码
ShowInGitHub 

`更新Xcode版本失效快捷修复`

```sh
$ curl https://raw.githubusercontent.com/cielpy/RPAXU/master/refreshPluginsAfterXcodeUpgrading.sh | sh
```

Dash		   最好用的阅读文档的工具
**ipa优化**
[Unused](https://github.com/jeffhodnett/Unused)       用来检查Xcode项目中没有用到的资源。
[Wasted](http://wasted.werk01.de/)	      选取ipa文件分析出哪些可以优化

**AppIcon自动生成**

IconKit  App应用图标制作工具

自动生成@3x,@2x, @1x 图片
1.RTImageAssets
2.ROTools
3.Prepo

**Mac自带工具**
FileMerge   代码比较工具
数码测色计    支持十六进制取色

### 轻量级文本编辑器
- 编辑器之神 vim
- 神之编辑器 Emacs   
- 编辑器之妖 TextMate
- 神级代码编辑器 Sublime Text

[一年成为Emacs高手(像神一样使用编辑器)](http://blog.csdn.net/redguardtoo/article/details/7222501)  
`开发者必备编辑器:`http://www.maczapp.com/zjbb-i-developers


### UI图片处理
GIF图录制  [liceCap](http://www.cockos.com/licecap/)
Pixelmator（图片处理）
轻量级修图软件，干净整洁的界面易于操作，界面清爽，布局灵活，熟悉 Photoshop 的人可迅速上手。
Logoist 2   一款非常不错的Mac平面设计软件

**测量工具** 
[Pixel Winch](http://www.ricciadams.com/projects/pixel-winch)
UI没标注肿么办，用直接Pixel Winch自动精确测量 UI 切图之间的距离吧，相比系统手工量方便多了



取色配色工具

Colored

快捷 App 启动和切换工具 :

 Manico



**图片压缩**
Tinyjpg      https://tinyjpg.com/
ImageOptim   https://imageoptim.com/　
色彩笔　　 　http://www.secaibi.com/tools/

More：http://www.iplaysoft.com/image-optimization-tools.html


UI设计工具推荐: http://tool.ui.cn/




**接口调试**
网络封包分析工具：[Charles](http://www.charlesproxy.com/)
调试API接口神器--- `Echo`、`Paw`、`do http`
Chrom插件 ---DHC 支持HTTP和HTTPS 、Postman
火狐插件 --- HttpRequester、HTTP Tool、RESTClient

在线接口测试工具
http://www.atool.org/httptest.php
http://www.haojson.com/postjson/



Json解析格式验证

http://json.cn/

http://json.parser.online.fr/beta/



**SQLite管理工具**
1.SQLite Professional  
2.MesaSQLite for Mac

**Api测试**
[聚合数据](https://www.juhe.cn/)
[百度ApiStore](http://apistore.baidu.com/)
Unicode转码
http://tool.chinaz.com/Tools/Unicode.aspx

**贝塞尔曲线生成**
http://cubic-bezier.com/

CheatSheet`推荐` 		长按`command`键可以弹出当前程序下的快捷键列表。
Better Rename  	文件批量重命名
Kaleidoscope 	   进行文本比较、图片比较的工具，可以支持任意类型文本格式的文件，也可以支持图片比较，无论你是可怜的码农，用作代码比较利器，文艺的作家，比较文章的不同，还是高端的设计师，来查看图片间的细微区别，都非常适合。
iTerm2           作为互联网从业者，装个系统终端增强工具总是必要的

Snap  		超级好用的应用快捷键启动工具



**推送调试**

**Knuff**--https://github.com/KnuffApp/Knuff

[友盟分享，极光推送Demo](http://www.cnblogs.com/sixindev/p/4545825.html)





思维导图/流程图

[百度脑图](http://naotu.baidu.com/)

https://www.processon.com/

https://www.mindmeister.com/zh

Mindnode



**下载工具**

Progressive Downloader  支持断点续传，多任务下载，速度也不错




**文件比较**

Beyond Compare   允许你快速和容易地比较你的文件和文件夹。通过使用简单的，强大的命令，你可以专注于你感兴趣的差异，并忽略其余。



程序开发相关工具：

Developer Font Tool 这是一款Mac平台的开发者字体工具，是一款帮助开发者和设计人员更简单的预览和比较字体效果的软件。

开发者HTTP工具 :

 Developer HTTP Tool 这是一款Mac的开发者HTTP工具，帮助开发者快速检查HTTP相关的参数：请求参数、请求内容。

`Mac 推荐`

QQ截图 http://snip.qq.com/

重命名namechanger  https://mrrsoftware.com/namechanger/

[Go2Shell](http://zipzapmac.com/go2shell)(官网)-从Finder直接打开终端



### 安卓开发

**Android Studio** -谷歌出的安卓开发环境，

**Eclipse/MyEclipse**

**TotalFinder**

也是必备的一个软件，给Finder加上像谷歌浏览器一样的标签页，在各个标签页切换很方便。



**KeyKey Typing Tutor** 

是一款 Mac OS X 上优秀的键盘打字练习工具，可以让我们快速的掌握高效率正确的键盘打字方式，软件界面也很精美！



**可视化文件对比合并同步工具**

**Araxis Merge**  

**Kaleidoscope** 

**Beyond Compare**



cakeBrew

Brew GUI工具



用分屏小工具

SizeUp 





更多：http://www.scoop.it/t/ios-dev

[iOS开发工具箱](https://github.com/iamjiyixuan/iOSDevToolBox/blob/master/README.md)

[75 Essential Tools for iOS Developers](http://benscheirman.com/2013/08/the-ios-developers-toolbelt/)






