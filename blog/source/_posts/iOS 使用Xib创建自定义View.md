---
title: iOS 使用Xib创建自定义View - CustomXibView
date: 2016-04-25 18:24:42
tags: iOS
categories: iOS
grammar_cjkRuby: true
---

#### 创建同名的UIView文件和Xib文件。

- 1、将Xib文件的`File's Owner` -> `Custom class` -> `Class`属性设置为同名的类。

- 2、将Xib文件的自定义View-> `Custom class` -> `Class`属性设置为同名的类。

  ​

- 注意:

  1、通过xib文件来自定义控件是，不会调用`init`,`initWithFrame:`方法；

  2、如果不指定自定义View的`Custom class`为同名类，则不会执行`initWithCoder`，`awakeFromNib`方法

  ​

#### XIB加载顺序

```objectivec
//TODO: NO.1 初始化
+ (CustomView *)instanceWithFrame:(CGRect)frame
{
    CustomView *view = (CustomView *)[[NSBundle mainBundle] loadNibNamed:@"CustomViewXib" owner:nil options:nil][0];
    view.frame = frame;
    return view;
}

//TODO: NO.2 初始化
+ (instancetype)customXiBView{
    NSString *className = NSStringFromClass([self class]);
    return [[[NSBundle mainBundle] loadNibNamed:className owner:self options:nil] firstObject];
}


//1、正在准备初始化
- (id)initWithCoder:(NSCoder *)aDecoder {
    self = [super initWithCoder:aDecoder];
    if (self) {
        
    }
    return self;
}

//2、初始化完毕`（若想初始化时做点事情，最好在这个方法里面实现）`
- (void)awakeFromNib {
    [super awakeFromNib];
}

//3、添加到父视图中
- (void)willMoveToSuperview:(UIView *)newSuperview {
    if (newSuperview) {
        
    }
}

//4、调整布局
- (void)layoutSubviews {
    [super layoutSubviews];
}
```

PS：在连线的时候,object一定要选择,你连线的view,而不是 `File's Owner`



[使用XIB 自定义UIView,并可AutoLayout](http://www.jianshu.com/p/bb65ff62e68c)

[[iOS UI] 如何通过xib创建自定义UIView并在xib中使用](http://www.jianshu.com/p/af7ec8483eea)

http://www.jianshu.com/p/7d59b9420bba

http://www.jianshu.com/p/371b1a195fb9

[init方法与xib区别](http://www.jianshu.com/p/56b34319ce76)

[自定义控件（通过代码或者xib文件）](http://www.jianshu.com/p/282eb033c40d)

[IOS Xib使用——Xib表示局部界面](http://www.cnblogs.com/jerehedu/p/4864039.html)

[通过 XIB 加载 UIView](http://honglu.me/2016/02/22/%E9%80%9A%E8%BF%87XIB%E5%8A%A0%E8%BD%BDUIView/)











