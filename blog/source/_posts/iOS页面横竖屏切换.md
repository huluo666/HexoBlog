---
title: iOS页面横竖屏自由切换适配
tags: iOS
categories:  iOS
date: 2017-04-20 13:50:18
grammar_cjkRuby: true
---

横竖屏控制加入以下3个函数即可

```objectivec
#pragma mark--禁止横屏
- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation
{
    return (toInterfaceOrientation == UIInterfaceOrientationPortrait);
}

- (BOOL)shouldAutorotate {
    return NO;//是否允许旋转
}

-(UIInterfaceOrientationMask)supportedInterfaceOrientations
{
    return UIInterfaceOrientationMaskPortrait;
    //只支持这一个方向(正常的方向)
}
```



这个方法可以实现，需要在根视图（nav，tabbar）和vc里重写

```objectivec
如果在UIViewController上，则在UIViewController对应的.m文件中加入三个函数即可。
如果在UITabBarController上，则在UITabBarController对应的.m文件中加入三个函数即可。
如果在UINavigationController上，则在UINavigationController对应的.m文件中加入三个函数即可。
```

页面基于UITabBarController使用自定义UITabBarController中加入

```objectivec
#pragma mark - - orientation
// 是否支持转屏
- (BOOL)shouldAutorotate
{
    return [self.selectedViewController shouldAutorotate];
}
// 返回nav栈中的最后一个对象支持的旋转方向
- (UIInterfaceOrientationMask)supportedInterfaceOrientations
{
    return [self.selectedViewController supportedInterfaceOrientations];
}
// 返回nav栈中最后一个对象,坚持旋转的方向
- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation
{
    return [self.selectedViewController preferredInterfaceOrientationForPresentation];
}
```

基于UINavigationController代码:

```objectivec
//旋转方向 默认竖屏
@property (nonatomic , assign) UIInterfaceOrientation interfaceOrientation;
@property (nonatomic , assign) UIInterfaceOrientationMask interfaceOrientationMask;

#pragma mark - - orientation
//设置是否允许自动旋转
- (BOOL)shouldAutorotate {
    return YES;
}

//设置支持的屏幕旋转方向
- (UIInterfaceOrientationMask)supportedInterfaceOrientations {
  return self.interfaceOrientationMask;
}

//设置presentation方式展示的屏幕方向
- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation {
  return self.interfaceOrientation;
}
```



```objectivec
- (void)viewWillAppear:(BOOL)animated
  {
    [super viewWillAppear:animated];
    //强制旋转竖屏
    navi.interfaceOrientation =   UIInterfaceOrientationLandscapeRight;
    navi.interfaceOrientationMask = UIInterfaceOrientationMaskLandscapeRight;

    //强制翻转屏幕，Home键在右边。
    [[UIDevice currentDevice] setValue:@(UIInterfaceOrientationLandscapeRight) forKey:@"orientation"];
    //刷新
    [UIViewController attemptRotationToDeviceOrientation];
}
```



2.部分页面需要全屏,其他页面禁止横屏

在appdelegate.h添加以下属性：

```
/***  是否允许横屏的标记 */
@property (nonatomic,assign)BOOL allowRotation;
```

appdelegate.m添加如下代码：

```objectivec
- (void)addNotificationCenter
{
    //在进入需要全屏的界面里面发送需要全屏的通知
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(startFullScreen) name:@"startFullScreen" object:nil];//进入全屏

    //在退出需要全屏的界面里面发送退出全屏的通知
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(endFullScreen) name:@"endFullScreen" object:nil];//退出全屏
}

#pragma mark 进入全屏
-(void)startFullScreen
{
    AppDelegate *appDelegate = (AppDelegate *)[[UIApplication sharedApplication] delegate];
    appDelegate.allowRotation = YES;
}

#pragma mark    退出横屏
-(void)endFullScreen
{
    AppDelegate *appDelegate = (AppDelegate *)[[UIApplication sharedApplication] delegate];
    appDelegate.allowRotation = NO;

//强制归正：
    if ([[UIDevice currentDevice]   respondsToSelector:@selector(setOrientation:)]) {
        SEL selector =     NSSelectorFromString(@"setOrientation:");
        NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:[UIDevice instanceMethodSignatureForSelector:selector]];
        [invocation setSelector:selector];
        [invocation setTarget:[UIDevice currentDevice]];
        int val =UIInterfaceOrientationPortrait;
        [invocation setArgument:&val atIndex:2];
        [invocation invoke];
    }
}

#pragma mark    禁止横屏
- (UIInterfaceOrientationMask )application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window
{
     //如果设置了allowRotation属性，支持全屏
    if (self.allowRotation) {
        return UIInterfaceOrientationMaskAll;
    }
    return UIInterfaceOrientationMaskPortrait;//默认全局不支持横屏
}
```



上面通过通知修改allowRotation属性来控制横竖屏幕

当然也可以通过AppDelegate单例直接修改allowRotation以控制横竖屏

```objectivec
 AppDelegate *appDelegate = (AppDelegate *)[[UIApplication sharedApplication] delegate];
appDelegate.allowRotation = YES;//yes支持横屏，No-不支持
```

在`viewWillAppear`和`viewWillDisappear`分别调用以上方法，在该控制器下即可顺利支持全屏。