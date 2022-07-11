---
title: iOS ViewController生命周期
date: 2016-04-07 13:12:51
tags: iOS,动画
grammar_cjkRuby: true
---

#### ViewController的生命周期

```objectivec
// 类的加载方法
// 是加载类的时候调用。也就是说，iOS应用启动时会加载所有的类，所有的类就都会调一次这个方法。
// 一个类的 `+load` 方法是在其父类的 `+load` 方法调用后再调用
// 一个`Category`的 `+load` 方法在被其扩展的类调完自有 `+load` 方法后才会调用
+ (void)load;

// 类的初始化方法
// 当某个类调用 alloc 方法时，将为实例在堆上分配空间时，调用一次initialize方法
// 而且该方法只调用一次，也就是说再次 alloc 操作的时候，不会再调用 initialize 这个方法了。
+ (instancetype)initialize;

```

更多：[objectivec +load vs +initialize](//http://blog.leichunfeng.com/blog/2015/05/02/objectivec-plus-load-vs-plus-initialize/)



**视图控制器push显示：**(执行顺序依次从上往下)

```objectivec

// 对象的初始化方法
- (instancetype)init;

// 加载视图
- (void)loadView;

// 视图加载完毕时调用
- (void)viewDidLoad;

// 视图将要显示的时候调用
- (void)viewWillAppear:(BOOL)animated;

// 视图中的子视图将要布局时调用
- (void)viewWillLayoutSubviews;

// 视图中的子视图布局完毕时调用
- (void)viewDidLayoutSubviews;

// 视图显示完毕的时候调用
- (void)viewDidAppear:(BOOL)animated;
```



**视图控制器pop消失**：

```objectivec
// 视图将要消失
-(void)viewWillDisappear:(BOOL)animated;

// 视图已经消失
-(void)viewDidDisappear:(BOOL)animated;

// 对象被释放前调用
-(void)dealloc;
```



**总结：**

```objectivec
alloc -> initWithNibName -> loadView -> viewDidLoad -> viewWillAppear -> viewDidAppear -> viewWillDisappear -> viewDidDisappear -> dealloc
```



**注意点：**

- 1、`viewWillAppear`

  > 其实在viewWillAppear这里改变UI元素不是很可靠，Autolayout发生在viewWillAppear之后，严格来说这里通常不做视图位置的修改，而用来更新Form数据。改变位置可以放在viewWilllayoutSubview或者didLayoutSubview里，而且在viewDidLayoutSubview确定UI位置关系之后设置autoLayout比较稳妥。另外，viewWillAppear在每次页面即将显示都会调用，viewWillLayoutSubviews虽然在lifeCycle里调用顺序在viewWillAppear之后，但是只有在页面元素需要调整时才会调用，避免了Constraints的重复添加。

引用：[iOS应用架构谈 view层的组织和调用方案](http://casatwy.com/iosying-yong-jia-gou-tan-viewceng-de-zu-zhi-he-diao-yong-fang-an.html)

- 2、`initWithNibName:bundle:`

> 1）在这个函数中应该只有相关数据的初始化，而且这些数据都是比较关键的数据，不要出现创建view的代码，也不要调`self.view`（`viewDidLoad`中view才加载到内存中），否则会导致ViewController创建view;View的创建留给后面的方法。
>
> 2）如果你是用 xib 创建 view 并初始化 ViewController，意味着你要使用 `initWithNibName:bundle:` 方法



- 3.`loadView`

> 1）添加subView应该在viewDidLoad里去做，此时不要实现自己的loadView
>
> 2）如果重写，一定要首先调用[super loadView],否则会循环调用loadView方法。调用[super loadView]一系列操作也会影响CPU性能，所以可以直接手动创建self.view = [[UIView alloc] initWithFrame:[UIScreen mainScreen].applicationFrame];
> 原因：loadView,其实是lazy load的,就是懒加载，每次访问UIViewController的view(比如controller.view、self.view)而且view为nil，loadView方法就会被调用。
> [super loadView] 方法会加载xib文件来创建UIViewController的view,如果没有xib则创建一个空白的View`self.view = [[UIView alloc] initWithFrame:[UIScreen mainScreen].applicationFrame]`;
> 如果你重写了这个方法,那么系统就会调用loadView,调用完后,再自动调用ViewDidLoad函数.这时就有可能造成死循环,所以非常不建议重写loadView方法。
> 当然在视图内存紧张情况，可适当考虑此方法，但经常出现内存警告时更应当从内存优化入手
> 3) 如果你是用 xib 创建 view 并初始化 ViewController，意味着你要使用 initWithNibName:bundle: 方法，则不要覆盖 loadView 方法。

`viewDidLoad`

由于view已经加载到内存中，所以`self.view !=nil`，我们可以随心所欲的添加`addSubview`,或者填充数据;



`-(void)viewWillAppear:(BOOL)animated`与`-(void)viewDidAppear:(BOOL)animated`

适合KVO的添加与移除等操作,切换前后台不会调用viewWillAppear.



###### `viewWillLayoutSubviews`调用情况分析

- init初始化不会触发layoutSubviews
- addSubview会触发layoutSubviews
- 设置view的Frame会触发layoutSubviews，当然前提是frame的值设置前后发生了变化
- 滚动一个UIScrollView会触发layoutSubviews
- 旋转Screen会触发父UIView上的layoutSubviews事件
- 改变一个UIView大小的时候也会触发父UIView上的layoutSubviews事件

