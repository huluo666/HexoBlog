---
title: iOS横竖屏设置
tags:
categories: iOS
date: 2017-08-30 15:24:35
grammar_cjkRuby: true
---

iOS指定页面默认横屏，自由切换

- model下，present方式推出界面。

- push横屏，带tabbar、navigation，且一个item下所有控制器对应的只有一个根navigation。


1、修改工程的info.plist中"Supported interface orientations"的值（一般在工程的Taget-> General -> Deployment Info -> Device Orientation处打钩来选择设备支持）

2、实现工程的AppDelegate文件中的（application:supportedInterfaceOrientationsForWindow:）方法，在此方法中返回程序支持的方向枚举。

iOS 横竖屏设置

```objectivec
UIInterfaceOrientationMaskLandscape 	 	支持左右横屏
UIInterfaceOrientationMaskAll  				支持四个方向旋转
UIInterfaceOrientationMaskAllButUpsideDown 	支持除了UpsideDown以外的旋转

//支持旋转
-(BOOL)shouldAutorotate{
   return YES;
}
//支持的方向
- (UIInterfaceOrientationMask)supportedInterfaceOrientations {
    return UIInterfaceOrientationMaskAllButUps
```



1、先在AppDelegate中重写`supportedInterfaceOrientationsForWindow`方法

```objectivec
-(UIInterfaceOrientationMask)application:(UIApplication*)application supportedInterfaceOrientationsForWindow:(UIWindow*)window{
   return	UIInterfaceOrientationMaskAll;	//支持全方向
}
```

2、创建一个UINavigationController的子类BaseNaviController，设置self.window的RootViewController为BaseNaviController;

然后，我们在BaseNaviController.m文件里面也实现着两个方法：

```objectivec
-(BOOL)shouldAutorotate
{
    return [self.topViewController shouldAutorotate];
}

-(UIInterfaceOrientationMask)supportedInterfaceOrientations
{
    return [self.topViewController supportedInterfaceOrientations];
}
```



3.在第一个页面写上，支持旋转 但是这个页面只支持竖屏，代码：

```objectivec
//支持旋转
-(BOOL)shouldAutorotate{
	return YES;
}

//支持的方向因为界面A我们只需要支持竖屏
- (UIInterfaceOrientationMask)supportedInterfaceOrientations {
	return UIInterfaceOrientationMaskPortrait;
}
```



4.第二个页面我们写上，我们所需要的横屏效果，代码：

当只需要锁定屏幕的旋转不需要设置屏幕的旋转方向 ( 方向固定是竖直)

```
// 是否支持屏幕旋转
-(BOOL)shouldAutorotate {
    return NO;
}
```

开启屏幕旋转并设置屏幕旋转支持的方向

```objectivec
// 是否支持屏幕旋转 (返回 NO 后面俩方法不调用，后面只支持竖直方向)
-(BOOL)shouldAutorotate {
    return NO;
}
// 支持屏幕旋转的方向
- (UIInterfaceOrientationMask)supportedInterfaceOrientations {
  //  通过设置返回的枚举值来改变屏幕旋转支持的方向
  //（iPad上的默认返回值是UIInterfaceOrientationMaskAll，
  //  iPhone上的默认返回值是UIInterfaceOrientationMaskAllButUpsideDown）
    return UIInterfaceOrientationMaskAll;
}

// Returns interface orientation masks. （返回最优先显示的屏幕方向）
// 同时支持Portrait和Landscape方向，但想优先显示Landscape方向，那软件启动的时候就会先显示Landscape，在手机切换旋转方向的时候仍然可以在Portrait和Landscape之间切换；
// 返回现在正在显示的用户界面方向
//此方法也仅有在当前viewController是rootViewController或者是modal模式时才生效.
- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation {

};
```



对于非modal模式的viewController:如果不是rootViewController,则重写supportedInterfaceOrientations,preferredInterfaceOrientationForPresentation以及shouldAutorotate方法, 按照当前viewController的需要返回响应的值.如果是rootViewController,则如下重写方法:

```
-(NSUInteger)supportedInterfaceOrientations{
        return self.topMostViewController.supportedInterfaceOrientations;
}
-(BOOL)shouldAutorotate{
        return [self.topMostViewController shouldAutorotate];
}
- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation{
return [self.topMostViewController preferredInterfaceOrientationForPresentation];
}
-(UIViewController*)topMostViewController{
//找到当前正在显示的viewController并返回.
}
```

显而易见, 我们巧妙的绕开了UIKit只调用rootViewController的方法的规则. 把决定权交给了当前正在显示的viewController.
对于modal模式的viewController. 则按照需要重写`supportedInterfaceOrientations`,
`preferredInterfaceOrientationForPresentation`以及`shouldAutorotate` 方法即可.





```
UIApplication* app = [UIApplication sharedApplication];

// 判断设备方向状态，做响应的操作
if(app.statusBarOrientation == UIInterfaceOrientationPortrait || app.statusBarOrientation == UIInterfaceOrientationPortraitUpsideDown){

}else if(app.statusBarOrientation == UIInterfaceOrientationLandscapeLeft || app.statusBarOrientation == UIInterfaceOrientationLandscapeRight){

}

UIInterfaceOrientationPortrait---------按钮在下
UIInterfaceOrientationPortraitUpsideDown---------按钮在上
UIInterfaceOrientationLandscapeLeft---------按钮在左
UIInterfaceOrientationLandscapeRight---------按钮在右


// 通过状态栏电池图标来判断屏幕方向
    if ([UIApplication sharedApplication].statusBarOrientation == UIInterfaceOrientationMaskPortrait) {
        // 竖屏 balabala
    } else {
        // 横屏 balabala
    }
```



AppDelegate.m中有如下方法可以设置整个app的方向。

```
- (UIInterfaceOrientationMask)application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window {
      return UIInterfaceOrientationMaskLandscape;
}

该方法实质上是制定window的方向，虽然一个app有多个VC，但是window都只有一个，所以这个指定了window的方向，那所有VC的方向就都是统一的。
```



已废弃方法

```
shouldAutorotateToInterfaceOrientation //在iOS6之前
```

http://www.jianshu.com/p/f917df3e3b2e

[视频播放器全屏旋转实现](http://www.jianshu.com/p/3e7724db8cd6)

https://github.com/JR-Dun/iOSRotationScreen

https://github.com/linsyorozuya/AutoRotateControlDemo

https://github.com/Jude309307972/RotateDemo

https://github.com/janicezhw/TestAutoRotation