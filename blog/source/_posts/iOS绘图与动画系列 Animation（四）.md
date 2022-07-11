---
title: iOS绘图与动画系列 Animation（四）
date: 2016-04-08 15:02:35
tags: iOS,动画,绘图
grammar_cjkRuby: true
---

本系列文章很多知识点来源[**ios核心动画高级技巧**](http://www.kancloud.cn/manual/ios)一书，提供了完整的绘图动画学习路线，强烈推荐阅读！

前面通过`Core Graphics（Quartz 2D）`和`CAShapeLayer`与`UIBezierPath`结合可以随心所欲的绘制我们想要的图形。

但是有些不足的是绘制的图形都是静态的，动画就是让这些图形动起来。



# 隐式动画、显式动画

![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/04/08/FossyAr_1dtpH2cbIMf5icP8XV8M144.png)

![](http://ww1.sinaimg.cn/large/65cc0af7gw1dxlusbklpmj.jpg)



#### 一、隐式动画

**1、什么是隐式动画？**

每一个UIView内部都默认关联着一个`CALayer`，我们可以称这个Layer为`Root Layer`(根层)，所有非`Root Layer`，也就是手动创建的CALayer对象，都存在着隐式动画

- 当对非RootLayer的部分属性进行修改时，默认会自动产生一些动画效果
- 而这些属性称为AnimatableProperties(可动画属性)
- 也就是说，对非根层的layer可动画属性进行修改产生的动画，就称为隐式动画
- 之所以叫隐式是因为我们并没有指定任何动画的类型。我们仅仅改变了一个属性，然后Core Animation来决定如何并且何时去做动画。
- 隐式动画是指通过UIView的`animateWithDuration:animations:`方法创建的动画。无须创建动画对象，只需改变动画层的属性，让核心动画自己去完成动画效果, 例如(CATransaction)。

```
bounds：用于设置CALayer的宽度和高度。修改这个属性会产生缩放动画
backgroundColor：用于设置CALayer的背景色。修改这个属性会产生背景色的渐变动画
position：用于设置CALayer的位置。修改这个属性会产生平移动画
```



**2、可以通过动画事务(CATransaction)关闭默认的隐式动画效果**

```swift
- (view)moveLayer:(BOOL)animation {
    if (animation) {
        // 改变了图层的位置，默认会有动画效果，这叫图层的隐式动画
        self.myLayer.position = CGPointMake(100, 100);
        [self.btnOfMove setTitle:@"还原" forState:UIControlStateNormal];
        return;
    }else {
        // 使用事务来去掉图层的隐式动画。
        [CATransaction begin];// 开启事务
        [CATransaction setDisableActions:YES];
        
        self.myLayer.position = CGPointMake(150, 150);
        
        [CATransaction commit];// 提交事务
    }
}
```

http://www.jianshu.com/p/966928e9cf49

隐式动画是ios4之后引入sdk的，之前只有显式动画。从官方的介绍来看，两者并没有什么差别，甚至苹果还推荐使用隐式动画，但是这里面有一个问题，就是使用隐式动画后，View会暂时不能接收用户的触摸、滑动等手势。这就造成了当一个列表滚动时，如果对其中的view使用了隐式动画，就会感觉滚动无法主动停止下来，必须等动画结束了才能停止。



#### 二、显式动画

显式动画是指用户自己通过`beginAnimations:context:`和`commitAnimations`创建的动画。

显示动画指的是，需要自己创建和管理动画对象，并且将它们应用到动画层，才能显示动画效果。

显式动画的基类为`CAAnimation`

`CAAnimation`分为这4种，他们分别是

> - **1.CABasicAnimation**
> - 通过设定起始点，终点，时间，动画会沿着你这设定点进行移动。
> - **2.CAKeyframeAnimation**
> - Keyframe顾名思义就是关键点的frame，你可以通过设定CALayer的始点、中间关键点、终点的frame，时间，动画会沿你设定的轨迹进行移动
> - **3.CAAnimationGroup**
> - Group也就是组合的意思，就是把对这个Layer的所有动画都组合起来。PS：一个layer设定了很多动画，他们都会同时执行，如何按顺序执行我到时候再讲。
> - **4.CATransition**
> - 这个就是苹果帮开发者封装好的一些动画。



1）属性动画---基本动画`CABasicAnimation`

```swift
CABasicAnimation *opAnim = [CABasicAnimation animationWithKeyPath:@opacity];
opAnim.duration = 1.0;
opAnim.fromValue = [NSNumber numberWithFloat:0.1];
opAnim.toValue= [NSNumber numberWithFloat:1.0];
opAnim.repeatCount = 1;
[view.layer addAnimation:opAnim forKey:@animateOpacity];
```

2）属性动画---关键帧动画`CAKeyframeAnimation`

```swift
//TODO:CAKeyframeAnimation关键帧动画
- (void)changeColor
{
    //1.创建CALayer对象
    CALayer *colorLayer = [CALayer layer];
    colorLayer.frame = CGRectMake(0, 0, 64, 64);
    colorLayer.position = CGPointMake(0, 150);
    colorLayer.backgroundColor = [UIColor greenColor].CGColor;
    [self.layer addSublayer:colorLayer];
    
    
    //2.创建关键帧动画
    CAKeyframeAnimation *animation = [CAKeyframeAnimation animation];
    animation.keyPath = @"backgroundColor";
    animation.duration = 2.0;
    animation.values = @[
                         (__bridge id)[UIColor blueColor].CGColor,
                         (__bridge id)[UIColor redColor].CGColor,
                         (__bridge id)[UIColor greenColor].CGColor,
                         (__bridge id)[UIColor blueColor].CGColor ];
    animation.keyTimes = @[@(0.0),@(0.3),@(0.6),@(1.0)];
    
    //3.将动画应用到colorLayer层
    [colorLayer addAnimation:animation forKey:nil];
}
```



3、动画组

```
//TODO:CAAnimationGroup动画组
- (void)changeColor2
{
    //1.创建CALayer对象
    CALayer *colorLayer = [CALayer layer];
    colorLayer.frame = CGRectMake(0, 0, 64, 64);
    colorLayer.position = CGPointMake(0, 150);
    colorLayer.backgroundColor = [UIColor greenColor].CGColor;
    [self.layer addSublayer:colorLayer];
    
    
    //创建动画1
    CAKeyframeAnimation *animation1 = [CAKeyframeAnimation animation];
    animation1.keyPath = @"position";
    animation1.path = bezierPath.CGPath;
    animation1.rotationMode = kCAAnimationRotateAuto;
   
    //创建动画2
    CABasicAnimation *animation2 = [CABasicAnimation animation];
    animation2.keyPath = @"backgroundColor";
    animation2.toValue = (__bridge id)[UIColor redColor].CGColor;
   
    
    //创建动画组
    CAAnimationGroup *groupAnimation = [CAAnimationGroup animation];
    groupAnimation.animations = @[animation1, animation2];
    groupAnimation.duration = 4.0;
    
    
    //将动画应用到Layer层
    [colorLayer addAnimation:groupAnimation forKey:nil];
}
```



4、过度动画`CATransition`

```swift
动画的开始与结束的快慢,有五个预置分别为(下同):
kCAMediaTimingFunctionLinear 线性,即匀速
kCAMediaTimingFunctionEaseIn 先慢后快
kCAMediaTimingFunctionEaseOut 先快后慢
kCAMediaTimingFunctionEaseInEaseOut 先慢后快再慢
kCAMediaTimingFunctionDefault 实际效果是动画中间比较快.
```

```swift
- (void)transitionForView:(UIView)view
{
    CATransition *caTransition = [CATransition animation];
    caTransition.duration = 0.5;
    caTransition.delegate = self;
    
    //设置运动速度  linear, easeIn, easeOut, easeInEaseOut, default
    caTransition.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseIn];//切换速度效果
    
    // kCATransitionFade, kCATransitionMoveIn kCATransitionPush, kCATransitionReveal
    caTransition.type = kCATransitionReveal;//动画切换风格
    
    //kCATransitionFromRight, kCATransitionFromLeft   kCATransitionFromTop, kCATransitionFromBottom
    //设置子类
    caTransition.subtype = kCATransitionFromLeft;//动画切换方向
    
    [self.layer addAnimation:groupAnimation forKey:nil];
}
```

`Block`块实现

```swift
 [UIView animateWithDuration:DURATION animations:^{
         [UIView setAnimationCurve:UIViewAnimationCurveEaseInOut];
         [UIView setAnimationTransition:transition forView:view cache:YES];
     }];
```


都有哪些`animationWithKeyPath`，如图

![](https://github.com/Coneboy-k/KKCoreAnimation/blob/master/image/2.png?raw=true)





参考：https://www.objc.io/issues/12-animations/animations-explained/

​	   http://www.apeth.com/iOSBook/ch17.html

