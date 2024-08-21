---
title: iOS绘图与动画系列 贝塞尔曲线UIBezierPath（二）
date: 2016-04-08 11:32:29
tags: iOS,动画,绘图
grammar_cjkRuby: true
---

**UIBezierPath简介**

UIBezierPath类是Core Graphics框架关于path的一个封装。可以定义简单的形状，如椭圆或者矩形，或者有多个直线和曲线段组成的形状。



**为什么要用UIBezierPath绘图。。？**

上文主要介绍了C语言的绘图方式，如果使用上文所说的C语言方式绘图代码就比较多，使用`贝塞尔路径`来实现绘图，使用起来比较方便,易阅读 ,善于管理,不像上面那种阅读起来比较麻烦，相对于C语言的Core Graphics来说更为平易近人。它能够使用ARC,所以今后绘图建议使用BezierPath。



**矩形圆角设置**（常用用于`UIIView`,`UIImageView`圆角设置）

```swift
typedef NS_OPTIONS(NSUInteger, UIRectCorner) {
    UIRectCornerTopLeft     = 1 << 0,//左上角圆角
    UIRectCornerTopRight    = 1 << 1,//右上角圆角
    UIRectCornerBottomLeft  = 1 << 2,//左下角圆角
    UIRectCornerBottomRight = 1 << 3,//右下角圆角
    UIRectCornerAllCorners  = ~0UL	 //全部圆角
};
```



**UIBezierPath对象对象创建方法**

```swift
// 创建基本路径
+ (instancetype)bezierPath;
// 创建矩形路径
+ (instancetype)bezierPathWithRect:(CGRect)rect;
// 创建椭圆路径
+ (instancetype)bezierPathWithOvalInRect:(CGRect)rect;
// 创建圆角矩形
+ (instancetype)bezierPathWithRoundedRect:(CGRect)rect cornerRadius:(CGFloat)cornerRadius; // rounds all corners with the same horizontal and vertical radius
// 创建指定位置圆角的矩形路径
+ (instancetype)bezierPathWithRoundedRect:(CGRect)rect byRoundingCorners:(UIRectCorner)corners cornerRadii:(CGSize)cornerRadii;
// 创建弧线路径
+ (instancetype)bezierPathWithArcCenter:(CGPoint)center radius:(CGFloat)radius startAngle:(CGFloat)startAngle endAngle:(CGFloat)endAngle clockwise:(BOOL)clockwise;
// 通过CGPath创建
+ (instancetype)bezierPathWithCGPath:(CGPathRef)CGPath;
```



**相关属性**

```swift
// 与之对应的CGPath
@property(nonatomic) CGPathRef CGPath;
- (CGPathRef)CGPath NS_RETURNS_INNER_POINTER CF_RETURNS_NOT_RETAINED;

// 是否为空，该值指示路径是否有任何有效的元素。
@property(readonly,getter=isEmpty) BOOL empty;
// 整个路径相对于原点的位置及宽高
@property(nonatomic,readonly) CGRect bounds;图形路径中的当前点
// 图形路径中的当前位置
@property(nonatomic,readonly) CGPoint currentPoint;

// 线宽
@property(nonatomic) CGFloat lineWidth;

// 终点连接类型
@property(nonatomic) CGLineCap lineCapStyle;
typedef CF_ENUM(int32_t, CGLineCap) {
    kCGLineCapButt,//斜角连接
    kCGLineCapRound,//圆滑衔接
    kCGLineCapSquare//斜角连接
};

// 交叉点的类型
@property(nonatomic) CGLineJoin lineJoinStyle;
typedef CF_ENUM(int32_t, CGLineJoin) {
    kCGLineJoinMiter,
    kCGLineJoinRound,
    kCGLineJoinBevel
};

// 两条线交汇处内角和外角之间的最大距离,需要交叉点类型为kCGLineJoinMiter是生效，最大限制为10,这个限制值有助于避免在连接线段志坚的尖峰
@property(nonatomic) CGFloat miterLimit;
// 个人理解为绘线的精细程度，默认为0.6，数值越大，需要处理的时间越长
@property(nonatomic) CGFloat flatness;
// 决定使用even-odd或者non-zero规则
@property(nonatomic) BOOL usesEvenOddFillRule;
```





```swift
//设置画笔起始点
- (void)moveToPoint:(CGPoint)point;
//从当前点到指定点绘制直线
- (void)addLineToPoint:(CGPoint)point;

//TODO: 添加贝塞尔曲线
//endPoint终点 controlPoint1、controlPoint2控制点
- (void)addCurveToPoint:(CGPoint)endPoint controlPoint1:(CGPoint)controlPoint1 controlPoint2:(CGPoint)controlPoint2;
//endPoint终点 controlPoint控制点
- (void)addQuadCurveToPoint:(CGPoint)endPoint controlPoint:(CGPoint)controlPoint;

//TODO: 添加弧线
// center弧线圆心坐标 radius弧线半径 startAngle弧线起始角度 endAngle弧线结束角度 clockwise是否顺时针绘制
- (void)addArcWithCenter:(CGPoint)center radius:(CGFloat)radius startAngle:(CGFloat)startAngle endAngle:(CGFloat)endAngle clockwise:(BOOL)clockwise NS_AVAILABLE_IOS(4_0);

//闭合路径，封闭未形成闭环的路径
- (void)closePath;

//移除所有的点，删除所有的subPath
- (void)removeAllPoints;

// 追加bezierPath路径
- (void)appendPath:(UIBezierPath *)bezierPath;
// 创建并返回一个新的反转内容的当前路径曲线路径的对象。
- (UIBezierPath *)bezierPathByReversingPath NS_AVAILABLE_IOS(6_0);

// 用指定的仿射变换矩阵变换路径的所有点
- (void)applyTransform:(CGAffineTransform)transform;

```

```swift
- (void)fill;		//填充
- (void)stroke;		//路径绘制
- (void)addClip;	//在这以后的图形绘制超出当前路径范围则不可见
```



**画图示例代码**

```swift
//TODO: 直线
- (void)drawRect2:(CGRect)rect
{
    // 设置线的填充色
    [[UIColor redColor] setStroke];

    // 新建一个bezier对象
    UIBezierPath *bezierPath = [UIBezierPath bezierPath];
    // 设置线宽度
    bezierPath.lineWidth = 10;
    // 设置线两头样式
    bezierPath.lineCapStyle = kCGLineCapRound;
    // 设置起点、终点坐标
    [bezierPath moveToPoint:CGPointMake(10, 10)];
    [bezierPath addLineToPoint:CGPointMake(100, 100)];
    // 开始绘制
    [bezierPath stroke];
}

//TODO: 二阶曲线
- (void)drawRect3:(CGRect)rect
{
    // 设置线的填充色
    [[UIColor redColor] setStroke];

    // 新建一个bezier对象
    UIBezierPath *bezierPath = [UIBezierPath bezierPath];
    // 设置线宽度
    bezierPath.lineWidth = 10;
    // 设置线两头样式
    bezierPath.lineCapStyle = kCGLineCapRound;
    // 设置起点、终点坐标
    [bezierPath moveToPoint:CGPointMake(100, 100)];
    [bezierPath addQuadCurveToPoint:CGPointMake(200, 200) controlPoint:CGPointMake(300, 0)];

    // 开始绘制
    [bezierPath stroke];
}


//TODO: 三阶曲线
- (void)drawRect4:(CGRect)rect
{
    // 设置线的填充色
    [[UIColor redColor] setStroke];

    // 新建一个bezier对象
    UIBezierPath *bezierPath = [UIBezierPath bezierPath];
    // 设置线宽度
    bezierPath.lineWidth = 10;
    // 设置线两头样式
    bezierPath.lineCapStyle = kCGLineCapRound;
    // 设置起点、终点坐标
    [bezierPath moveToPoint:CGPointMake(100, 100)];
    [bezierPath addCurveToPoint:CGPointMake(200, 200) controlPoint1:CGPointMake(150, 0) controlPoint2:CGPointMake(150, 300)];

    // 开始绘制
    [bezierPath stroke];
}

//TODO: 多阶曲线
- (void)drawRect5:(CGRect)rect
{
    // 设置线的填充色
    [[UIColor redColor] setStroke];

    // 新建一个bezier对象
    UIBezierPath *bezierPath = [UIBezierPath bezierPath];
    // 设置线宽度
    bezierPath.lineWidth = 10;
    // 设置线两头样式
    bezierPath.lineCapStyle = kCGLineCapRound;
    // 设置起点、终点坐标
    [bezierPath moveToPoint:CGPointMake(100, 100)];
    [bezierPath addCurveToPoint:CGPointMake(200, 200)
                  controlPoint1:CGPointMake(150, 0)
                  controlPoint2:CGPointMake(150, 300)];

    // 创建第二条贝塞尔曲线
    UIBezierPath *bezierPath2 = [UIBezierPath bezierPath];
    // 设置起点、终点坐标
    [bezierPath2 moveToPoint:CGPointMake(200, 200)];
    [bezierPath2 addCurveToPoint:CGPointMake(290, 290)
                   controlPoint1:CGPointMake(250, 0)
                   controlPoint2:CGPointMake(250, 300)];
    // 将第二条线，加到第一条线上面去
    [bezierPath appendPath:bezierPath2];

    // 开始绘制
    [bezierPath stroke];
}


#pragma mark---
//TODO: 画矩形
- (void)drawRect10:(CGRect)rect
{
    // 设置线的填充色
    [[UIColor redColor] setStroke];

    // 新建一个bezier对象，此对象用于绘制矩形，需要传入绘制的矩形的Frame
    UIBezierPath *bezierPath = [UIBezierPath bezierPathWithRect:CGRectMake(10, 10, 280, 280)];
    // 设置线宽度
    bezierPath.lineWidth = 10;
    // 设置线两头样式
    bezierPath.lineCapStyle = kCGLineCapRound;

    // 开始绘制
    [bezierPath stroke];
}

//TODO: 2:画矩形，圆角矩形
- (void)drawRect11:(CGRect)rect
{
    // 设置线的填充色
    [[UIColor redColor] setStroke];

    // 新建一个bezier对象，此对象用于绘制一个圆角矩形
    UIBezierPath *bezierPath = [UIBezierPath bezierPathWithRoundedRect:CGRectMake(10, 10, 280, 280)
                                                          cornerRadius:30];
    // 设置线宽度
    bezierPath.lineWidth = 10;

    // 开始绘制
    [bezierPath stroke];
}


/**
 *  rect: 需要画的矩形的Frame
 *  corners: 哪些部位需要画成圆角
 *  cornerRadii: 圆角的Size
 */
//TODO: 画矩形，部分圆角的矩形
- (void)drawRect12:(CGRect)rect
{
    // 设置线的填充色
    [[UIColor redColor] setStroke];

    // 新建一个bezier对象，此对象用于绘制一个部分圆角的矩形，左上、右下
    UIBezierPath *bezierPath = [UIBezierPath bezierPathWithRoundedRect:CGRectMake(10, 10, 280, 280)
                                                     byRoundingCorners:UIRectCornerTopLeft | UIRectCornerBottomRight
                                                           cornerRadii:CGSizeMake(10, 10)];
    // 设置线宽度
    bezierPath.lineWidth = 10;

    // 开始绘制
    [bezierPath stroke];
}


//TODO: 画圆
- (void)drawRect13:(CGRect)rect
{
    // 设置线的填充色
    [[UIColor redColor] setStroke];

    // 新建一个bezier对象，此对象用于绘制内切圆，需要传入绘制内切圆的矩形的Frame
    UIBezierPath *bezierPath = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(10, 10, 280, 280)];
    // 设置线宽度
    bezierPath.lineWidth = 10;
    // 设置线两头样式
    bezierPath.lineCapStyle = kCGLineCapRound;

    // 开始绘制
    [bezierPath stroke];
}



/**
 *  center: 圆心坐标
 *  radius: 圆的半径
 *  startAngle: 绘制起始点角度
 *  endAngle: 绘制终点角度
 *  clockwise: 是否顺时针
 */
//TODO: 圆弧
- (void)drawRect14:(CGRect)rect
{
    // 设置线的填充色
    [[UIColor redColor] setStroke];

    // 新建一个bezier对象，此对象用于绘制一个圆弧
    UIBezierPath *bezierPath = [UIBezierPath bezierPathWithArcCenter:CGPointMake(150, 150)
                                                              radius:110
                                                          startAngle:0
                                                            endAngle:M_PI_2
                                                           clockwise:NO];
    // 设置线宽度
    bezierPath.lineWidth = 10;

    // 开始绘制
    [bezierPath stroke];
}

```



**`工具推荐`**

[https://github.com/YouXianMing/Tween-o-Matic-CN](https://github.com/YouXianMing/Tween-o-Matic-CN)这是个自定义贝塞尔曲线值的mac工具

由于图像绘制代码一般较多，下面这些工具可以 简单快速生成绘制代码，实时查看效果，简单高效，生成的代码也很规范。

1、`paintCode`

> PaintCode是由来自斯洛伐克首都伯拉第斯拉瓦的[**PixelCut**](http://www.pixelcut.com/)软件公司推出的，一款面向iOS和Mac应用开发者及设计师的矢量图形可视化开发工具。通过PaintCode，即使是没有编程经验的设计师也能绘制出精美的控件、图标及其他UI元素。而PaintCode最为显著的一点就是能够直接生成适用于iOS的objectivec或Swift的代码，节省了大量的编程时间，也正因如此，许多开发者将其称为设计与开发通吃的代码神器。

2、`QuartzCode`

官网：http://www.quartzcodeapp.com/

> QuartzCode 是一个快速的、 轻量级的、 强大的动画工具，转换矢量绘图和动画到Object C 和 Swift 代码。我们只需更改属性 ，还可以可以循环在几秒钟内，实时看到动画的变化。减少了在 Xcode 创建动画的障碍 ！

3、`Sketch` 	  矢量图设计工具

http://www.sketchcn.com/

