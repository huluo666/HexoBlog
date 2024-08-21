---
title: iOS 多环境配置 Build Configuration
tags: iOS
date: 2016-03-31 23:19:32
categories: [iOS]
grammar_cjkRuby: true
---

对于有些企业来说，应用在发布到 App Store 之前, 会把应用通过Ad Hoc 形式进行公测。目的是用来模拟真实用户的操作行为和流程,以此来找到一些更不容易发现的 Bug。

产品，测试，都需要自己打包，而且需要打不同环境的多个包，打包需要对配置进行修改，产品还要求公测包标题和图标需要带有Beta字样，这样改了改去非常麻烦而且非常耗费时间。


打包主要分为Debug, Beta, AppStore 这样的版本

其实XCode提供了一个非常容易使用的机制解决这些问题: Build Configuration

最终的目标是:
1、每个环境有一个单独的配置文件。例如我们可以改变测试服务器和生产服务器的URL。
2、有不同的包标识符,为每个环境设置包名称和应用程序图标。 这样做确保您可以直接识别哪个版本是安装在一个设备上。也许更重要的是,它还可以让我们在设备上有多个版本,因为每个环境都有自己的包标识符。
3、有一个预处理器宏能够区分不同环境。 
​    
当您创建一个新项目,Xcode已经提供了两种配置的: release 和 Debug


### 创建一个功能，设置不同BundleID 
不同版本要有不同的Bundle ID,比如
 1. com.mycompany.myapp.debug -- Debug版本
 2. com.mycompany.myapp.inhouse -- (Ad Hoc, Beta) 版本
 3. com.mycompany.myapp ---App Store(Production) 版本


多环境配置步骤
### 一、创建Build Configurations
默认Xcode会提供2个Build配置项(Build Configuration):Debug和Release。一般来说这样两种情况就足够了，但是在有些时候我们需要添加一个或多个新的配置项，添加一个新的配置项的步骤如下：

1）、方式：选中PROJECT的名称，然后选中Info，点击Configurations下面的`+`选择`Duplicate "Debug"Configuration` 生成一个新Configuration,双击重命名configuration ，修改如下图：
![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/03/31/FgWRnjX0WKEYIjRoBkmlheyMzfxN479.png)



### 二、创建 User-Defined Setting
在Xcode中使用User-Defined Setting可以定义一些Xcode编译使用的宏配置，以便我们为不同的配置能够分配不同的包ID，图标名称或AppID。

1、开启User-Defined Setting，如下图：
![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/03/31/Frvrx6jiUF9AzXTw3QHWlrRHppQc479.png)


2、在这个例子中，我创建了2个不同的用户自定义设置; `APP_BUNDLE_ID (APP Bundle ID)`和`APP_DISPLAY_NAME(APP的名称)`。

添加User-Defined Setting后，编辑里面每个设置构建配置值
效果如下图：

![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/03/31/FlOrq2HK2R75jqp4mEJisoQvPwDh479.png)


3.App Icon 设置
在 Media Assets 中 (在项目中的默认名字应该是 Images.xcassets), 点击菜单 “Editor” -> “New App Icon”, 建立两个新的 App Icon, 分别对应 Debug 和 Staging 版本. 然后把之前准备好的图标分别拖进去. 如图: 
![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/03/31/FufHtzkg6spZBKdUwIUBkprxsY2P479.png)

点击AppIcon添加管理AppICon
![添加图标1](http://7xr7vj.com1.z0.glb.clouddn.com/2016/04/01/FkLqdrqhWcE7cGr16Ryv8Dkl7Wul758.png)

![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/04/01/Fv9EinlyuD1uZIt_FUpky3YPpg3P758.png)


### 三、修改Info.plist文件
所有在上一步中定义的User-Defined Setting将在项目的info.plist中使用。
修改您的info.plist，通过插入用户自定义设置中定义的值。

Bundle identifiter(捆绑标识符)：`$(APP_BUNDLE_ID)`
Bundle display name(包显示名称)：演示应用`$(APP_DISPLAY_NAME)`

![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/03/31/FiV6Qzk3xYlQPXEWBu5cSSFKz_TV513.png)


设置完毕后，可进行Run测试，Debug,Beta，Release 编译后进入手机桌面，该工程将会有不同图标，名称的App。
![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/04/01/FhBRLfhncgDOQS6a1QgicO2SI9Qr445.jpeg)



`设置多个Scheme`
当然也可以设置多个Scheme进行管理`shift+cmmand+<`,勾选shared以便git同步
[](http://7xr7vj.com1.z0.glb.clouddn.com/2016/04/01/FhBRLfhncgDOQS6a1QgicO2SI9Qr445.jpeg)



**测试相关信息**

1：获取`bundle Id`信息：[[NSBundle mainBundle] bundleIdentifier]；

2：获取版本号：[[[NSBundle mainBundle]infoDictionary] objectForKey:@"CFBundleVersion"];

3: 获取App显示名：[[[NSBundle mainBundle]infoDictionary] objectForKey:@"CFBundleDisplayName"];




参考网址：
http://appfoundry.be/blog/2014/07/04/Xcode-Env-Configuration/       
http://ravelantunes.com/blog/xcode-build-process/           
http://www.teratotech.com/blog/xcode-7-steps-to-easily-switch-between-multiple-environments/




