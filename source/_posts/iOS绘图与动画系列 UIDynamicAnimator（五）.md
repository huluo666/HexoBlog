---

title: iOS绘图与动画系列 UIDynamicAnimator（五）
date: 2016-04-08 15:02:35
tags: iOS,动画,绘图
grammar_cjkRuby: true

---

iOS 7 中推出的UIKit Dynamics，主要带来了模拟现实的二维动画效果，用来创建逼真的物理动画。

![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/04/12/Fna6-VRKt81gIKaqC7GujwC4Zrfi469.png)

## 关键类

- **`UIDynamicAnimator`**	封装了底层 iOS 物理引擎，为动力项（UIDynamicItem）提供物理相关的功能和动画。
- **`UIDynamicBehavior`**     动力行为，为动力项提供不同的物理行为
- **`UIDynamicItem`**              动力项，相当于现实世界中的一个基本物体





`UIDynamicAnimator`常用API

```objectivec
@interface UIDynamicAnimator: NSObject

// 传入一个 Reference view 创建一个 Dynamic Animator
- (instancetype)initWithReferenceView:(UIView *)view;
// 添加动力行为
- (void)addBehavior:(UIDynamicBehavior *)behavior;
// 删除指定的动力行为
- (void)removeBehavior:(UIDynamicBehavior *)behavior;
// 删除所有的动力行为
- (void)removeAllBehaviors;

//关联view
@property (nullable, nonatomic, readonly) UIView *referenceView;
// 获取所有的 Behaviors
@property (nonatomic, readonly, copy) NSArray<__kindof UIDynamicBehavior*> *behaviors;

// 获取在 CGRect 内所有的动力项，这个 CGRect 是基于 Reference view 的二维坐标系统的
- (NSArray<id<UIDynamicItem>> *)itemsInRect:(CGRect)rect;

// 如果动力项不是通过 animator 自动计算改变状态，比如，通过代码强制改变一个 item 的 transfrom 时，可以用这个方法通知 animator 这个 item 的改变。如果不用这个方法，animator 之后的动画会覆盖代码中对 item 做的改变，相当于代码改变 transform 变得没有意义。
- (void)updateItemUsingCurrentState:(id <UIDynamicItem>)item;

// 是否正在运行
@property (nonatomic, readonly, getter = isRunning) BOOL running;

// 已经运行了多久的时间
- (NSTimeInterval)elapsedTime;

// delegate 中有两个回调方法，一个是在 animator 暂停的时候调用，一个是在将要恢复的时候调用
@property (nullable, nonatomic, weak) id <UIDynamicAnimatorDelegate> delegate;

@end
```



`UIDynamicBehavior` 的主要方法和属性

```objectivec
// 在将要进行动画时的 block 回调
@property(nonatomic, copy) void (^action)(void)

// 添加到该动态行为中的子动态行为
@property(nonatomic, readonly, copy) NSArray *childBehaviors

//  该动态行为相关联的dynamicAnimator
@property(nonatomic, readonly) UIDynamicAnimator *dynamicAnimator

//添加一个子动态行为
- (void)addChildBehavior:(UIDynamicBehavior *)behavior

// 移除一个子动态行为
- (void)removeChildBehavior:(UIDynamicBehavior *)behavior

// 当该动态行为将要被添加到一个UIDynamicAnimator中时，这个方法会被调用。
- (void)willMoveToAnimator:(UIDynamicAnimator *)dynamicAnimator
```



https://github.com/ZhipingYang/UIKitDynamics

https://github.com/xiaofei86/UIKitDynamics

http://vit0.com/blog/2014/03/08/ios-7-uikit-dynamic-xue-xi-zong-jie/