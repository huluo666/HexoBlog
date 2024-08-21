---
title: iOS绘图与动画系列 Core Graphics与Quartz 2D（一）
date: 2016-04-06 14:40:43
tags: iOS,动画,绘图
grammar_cjkRuby: true
---

iOS绘图Core Graphics与Quartz 2D

**Quartz Core, Core Graphics and Quartz 2D之间的区别**
[StackFlow](http://stackoverflow.com/questions/1877987/whats-the-difference-between-quartz-core-core-graphics-and-quartz-2d)

### Quartz frameworks 及其 APIs

`CoreGraphics.framework`

`Core Graphics`是一套C语言 API框架， 支持向量图形，线、形状、图案、路径、剃度、位图图像和pdf 内容的绘制。我们平时常见的各种UIKit框架提供的UI控件，实际上都是由Core Graphics进行绘制的。

- [Quartz 2D](https://developer.apple.com/library/mac/#documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/Introduction/Introduction.html#//apple_ref/doc/uid/TP30001066) 指一组二维绘图与渲染的 API。它是Core Graphics框架的一部分， CoreGraphics 有时候也被成为 Quartz 2D。因此我们一般都是用CGxxxx命名的函数进行绘图。

  **Quartz 2D支持功能**：Quartz是资源和设备无关的,提供基本路径绘制，剃度填充图案，图像，透明绘制和透明层、遮蔽和阴影、颜色管理，坐标转换，字体、offscreen呈现、pdf文档创建、显示和分析等功能。

  具体可实现功能如：

  ```wiki
  绘制图形:线条、三角形、矩形、圆、弧等
  绘制文字
  绘制\生成图片(图像)
  读取\生成PDF
  截图\裁剪图片
  自定义UI控件 (iOS中大部分控件的内容都是通过Quartz2D画出来的)
  ```

  官方Demo:[example code](http://developer.apple.com/library/ios/samplecode/QuartzDemo/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007531)。

  ​

`QuartzCore.framework`

- [Core Animation](https://developer.apple.com/library/mac/documentation/GraphicsImaging/Conceptual/Animation_Overview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004952): objectivec API 做二维动画。.

- [Core Image](https://developer.apple.com/library/mac/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_intro/ci_intro.html#//apple_ref/doc/uid/TP30001185): 图像和视频处理（滤镜，扭曲，转换）。iOS 5

  ​

  **下面列出了Quartz 2D包含的数据类型**：

- CGPathRef：用于向量图，可创建路径，并进行填充或描画(stroke)

- CGImageRef：用于表示bitmap图像和基于采样数据的bitmap图像遮罩。

- CGLayerRef：用于表示可用于重复绘制(如背景)和幕后(offscreen)绘制的绘画层

- CGPatternRef：用于重绘图

- CGShadingRef、CGGradientRef：用于绘制渐变

- CGFunctionRef：用于定义回调函数，该函数包含一个随机的浮点值参数。当为阴影创建渐变时使用该类型

- CGColorRef, CGColorSpaceRef：用于告诉Quartz如何解释颜色

- CGImageSourceRef,CGImageDestinationRef：用于在Quartz中移入移出数据

- CGFontRef：用于绘制文本

- CGPDFDictionaryRef, CGPDFObjectRef, CGPDFPageRef, CGPDFStream, CGPDFStringRef, and CGPDFArrayRef：用于访问PDF的元数据

- CGPDFScannerRef, CGPDFContentStreamRef：用于解析PDF元数据

- CGPSConverterRef：用于将PostScript转化成PDF。在iOS中不能使用。

  ​


**绘图创建步骤（重写drawRect函数）**

```wiki
1、我们首先需要开启图形上下文`CGContextRef ctx = UIGraphicsGetCurrentContext();
   //为C语言，创建对象无需加*`。
2、然后对我们想绘制的图片或者图形进行一系列的操作（添加路径，设置属性等）。
3、最后就是渲染在我的View上面。
```



##### 图像的重绘（刷帧）

调用重绘view对象的

```
- (void)setNeedsDisplay; 刷新全部view
- (void)setNeedsDisplayInRect:(CGRect)rect; 刷新区域为rect的view
```

#### drawRect方法使用注意点

- 在iOS开发中不允许开发者直接调用`drawRect:`方法，刷新绘制内容需要调用`setNeedsDisplay`方法。
- 若使用CALayer绘图，只能在drawInContext: 中（类似于drawRect）绘制，或者在delegate中的相应方法绘制。同样也是调用setNeedDisplay等间接调用以上方法
- 若要实时画图，不能使用gestureRecognizer，只能使用touchbegan等方法来调用setNeedsDisplay实时刷新屏幕
- 凡是“UI”开头的相关绘图函数，都是UIKit对`Core Graphics`的封装



### 画图事例

**常用属性**

>  设置线段的宽度 				        	 `CGContextSetLineWidth`
>  设置线段颜色  					   	 `CGContextSetRGBStrokeColor`
>  设置起点和终点样式（圆角，直角等）       `CGContextSetLineCap`
>  设置连接点样式（圆角，尖角等） 		 `CGContextSetLineJoin`
>  设置竖线  							 `CGContextSetLineDash`
>  填充颜色（实心） 				         `CGContextSetRGBFillColor` `[[UIColor redColor]setFill]`
>
>  **画虚线的参数**：
>  phase：相位，虚线的起始位置＝通常使用 0 即可，从头开始画虚线
>  lengths:长度的数组
>  count ： lengths 数组的个数
>  CGFloat lengths[2] = {20.0,10.0};
>  CGContextSetLineDash(context, 0, lengths, 3);



**主要渲染填充模式CGPathDrawingMode**

```
kCGPathStroke:画线（空心），只有边框
kCGPathFill:  填充（实心），只有填充（非零缠绕数填充），不绘制边框
kCGPathEOFill:奇偶规则填充（多条路径交叉时，奇数交叉填充，偶交叉不填充）
kCGPathFillStroke：既有边框又有填充
kCGPathEOFillStroke：奇偶填充并绘制边框
```

**官方Demo演示图**
![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/04/07/Ftet5exO40m3uAl3Got9CG8FiCVE193.gif)

![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/04/07/Fs-Y6ZgkBaF0h9SCnlsJ3zwWdZ0E193.gif)

![](http://7xr7vj.com1.z0.glb.clouddn.com/2016/04/07/FttRA8-Ss-ClMWZANxz-QOBIdTqo193.gif)


**渲染方式写法**

```
1. CGContextDrawPath(context, kCGPathStroke);
2. CGContextStrokePath(ctx);
```

**主要画图Api**

```

直线
CGContextAddLineToPoint
椭圆
CGContextAddEllipseInRect
矩形
CGContextAddRect
圆
CGContextAddArc
```



**下面所列举C语言和OC画图方式，OC绘图方式为UIBezierPath**

- 方式一:C语言的方式

```
 - (void)drawRect:(CGRect)rect
  {
      CGContextRef ctx = UIGraphicsGetCurrentContext();
      CGContextMoveToPoint(ctx,50, 50);
      CGContextAddLineToPoint(ctx, 110, 120);
      CGContextAddLineToPoint(ctx, 150, 40);
      CGContextSetLineWidth(ctx, 10);
      CGContextStrokePath(ctx);
  }
```

- 方式二:C语言的方式之path

```swift
  - (void)drawRect:(CGRect)rect
  {
      CGContextRef ctx = UIGraphicsGetCurrentContext();

      // 获取路径
      CGMutablePathRef path = CGPathCreateMutable();
      CGPathMoveToPoint(path, NULL, 50, 50);
      CGPathAddLineToPoint(path, NULL, 110, 120);
      // 将路径添加到上下文中
      CGContextAddPath(ctx, path);

	  CGContextStrokePath(ctx)
  }
```

- 方式三:UIBezierPath

```swift
  - (void)drawRect:(CGRect)rect
  {
      CGContextRef ctx = UIGraphicsGetCurrentContext();

      UIBezierPath *path = [[UIBezierPath alloc]init];
      [path moveToPoint:CGPointMake(50, 50)];
      [path addLineToPoint:CGPointMake(110, 120)];
      CGContextAddPath(ctx, path.CGPath);

      CGContextStrokePath(ctx);
  }
```



- 方式四:C(CGPath) + OC(UIBezierPath)

```
- (void)drawRect:(CGRect)rect
  {
      CGContextRef ctx = UIGraphicsGetCurrentContext();

      CGMutablePathRef path = CGPathCreateMutable();
      CGPathMoveToPoint(path, NULL, 50, 50);
      CGPathAddLineToPoint(path, NULL, 110, 120);

      // 把c的路径CGPath转成oc的路径bezierPath
      UIBezierPath *path1 = [UIBezierPath bezierPathWithCGPath:path];
      [path1 addLineToPoint:CGPointMake(80, 80)];

      // 把路径添加到上下文中
      CGContextAddPath(ctx, path1.CGPath);

      CGContextStrokePath(ctx);

  }
```

- 方式五:纯OC实现(UIBezierPath)

```
 - (void)drawRect:(CGRect)rect
  {
      UIBezierPath *path = [UIBezierPath bezierPath];
      [path moveToPoint:CGPointMake(50, 50)];
      [path addLineToPoint:CGPointMake(110, 120)];
      [path stroke];
  }
```



**直线**

两点一线，  `x0=x1&&y0！=y1` 垂直线   `x0！=x1&&y0=y1`水平线

起点  `CGContextMoveToPoint(ctx,x0,y0);`

终点 `CGContextAddLineToPoint(ctx,x1,y1);`

```swift
//TODO: 画直线
-(void)drawLine
{
    //获得当前画板（图形上下文对象）为C语言，创建对象无需加*
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    //!!!: 相关属性设置 可选
    //设置线条的颜色
    //CGContextSetRGBStrokeColor(ctx, 0.2, 0.2, 0.2, 1.0);
    CGContextSetStrokeColorWithColor(ctx, [UIColor redColor].CGColor);
    CGContextSetLineWidth(ctx, 10); // 设置线段的宽度
    CGContextSetLineJoin(ctx, kCGLineJoinRound); // 设置线段起点和终点的样式都为圆角
    CGContextSetLineCap(ctx, kCGLineCapRound);   // 设置线段的转角样式为圆角

    //开始画线, x，y为开始点的坐标
    CGContextMoveToPoint(ctx, 0, 40);
    //画直线, x，y为线条结束点的坐标
    CGContextAddLineToPoint(ctx, self.bounds.size.width,40);
    CGContextStrokePath(ctx);//渲染，绘制出一条空心的线段
}
```





**三角形**

三角形是由直线组成，画线方法与直线一样，最少3个点，添加三个点然后闭合路径`CGContextClosePath`

`CGContextMoveToPoint(triangle, 0, 0)`; // 设置起点1
`CGContextAddLineToPoint(triangle, 100, 200)`; // 设置第二个点2
`CGContextAddLineToPoint(triangle,200, 0)`; // 设置第三个点3

```swift
//TODO: 三角形
- (void)drawTriangle
{
    CGContextRef triangle = UIGraphicsGetCurrentContext(); // 获得图形上下文
    CGContextMoveToPoint(triangle, 150, 40); // 设置起点
    CGContextAddLineToPoint(triangle, 60, 200); // 设置第二个点
    CGContextAddLineToPoint(triangle,240, 200); // 设置第三个点
    CGContextClosePath(triangle); // 关闭起点和终点
    CGContextStrokePath(triangle); // 渲染，绘制出三角形
}
```



**矩形**

设置一个CGRect大小可以了`CGContextAddRect(CGContextRef ctx, CGRect rect)`

```swift
//TODO: 矩形
-(void)drawaRect
{
    //获得当前画板
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    //颜色
    CGContextSetRGBStrokeColor(ctx, 0.2, 0.2, 0.2, 1.0);
    //画线的宽度
    CGContextSetLineWidth(ctx, 0.25);
    CGContextAddRect(ctx, CGRectMake(2, 2, 30, 30));
    CGContextStrokePath(ctx);
}
```



**圆`CGContextAddArc`**

```swift
/**
 *  画圆
 *  @param ctx        上下文
 *  @param x          圆起点坐标X
 *  @param y          圆起点坐标y
 *  @param radius     圆的半径
 *  @param startAngle 起始角度
 *  @param endAngle   结束角度
 *  @param clockwise  顺逆时针
 */
CGContextAddArc(CGContextRef ctx, CGFloat x, CGFloat y,CGFloat radius, CGFloat startAngle, CGFloat endAngle, int clockwise)
```



```swift
//TODO: 画圆
- (void)drawRoundRect
{
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    CGContextSetRGBStrokeColor(ctx, 0.2, 0.2, 0.2, 1.0);//颜色
    CGContextSetLineWidth(ctx, 0.25);//画线的宽度

    CGPoint center = CGPointMake(self.bounds.size.width / 2, self.bounds.size.height / 2);//设置圆心位置
    CGFloat radius = self.bounds.size.height / 2;//设置半径
    CGFloat startAngle = - M_PI_2;//圆起点位置
    CGFloat endAngle = - M_PI_2 +  M_PI * 2;//圆终点位置
    CGContextAddArc(ctx,center.x,center.y,radius,startAngle, endAngle, 0);

    CGContextDrawPath(ctx, kCGPathStroke); //绘制路径
}

```



### 环形

环形其实就是有一定宽度边框圆，所以我们设置一定的线宽即可如：`CGContextSetLineWidth(ctx, 10);`

环形是空心圆，所以`CGContextDrawPath(ctx, kCGPathStroke);`

绘制方式选择kCGPathStroke只画线，不填充，填充那就是个实心圆了

```swift
//TODO: 环形
-(void)drawAnnular
{
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    CGPoint center = CGPointMake(self.bounds.size.width / 2, self.bounds.size.height / 2);//设置圆心位置
    CGFloat radius = self.bounds.size.height / 2;//设置半径
    CGFloat startAngle = - M_PI_2;//圆起点位置
    CGFloat endAngle = - M_PI_2 +  M_PI * 2;//圆终点位置
    CGContextAddArc(ctx,center.x,center.y,radius-5,startAngle, endAngle, 0);
    CGContextSetLineWidth(ctx, 10);
    [[UIColor greenColor]set];
    CGContextDrawPath(ctx, kCGPathStroke); //绘制路径
}
```



**圆弧**

绘制圆弧 就是一个圆形的一部分

```swift
-(void)circularArc
{
    CGContextRef context = UIGraphicsGetCurrentContext();
    CGPoint center = CGPointMake(self.bounds.size.width / 2, self.bounds.size.height / 2);//设置圆心位置
    CGFloat radius = self.bounds.size.height / 2;//设置半径
    CGFloat startAngle = - M_PI_2;//圆起点位置
    CGFloat endAngle = - M_PI_2 +  M_PI*1.2;//圆终点位置
    CGContextAddArc(context, center.x,center.y,radius,startAngle, endAngle, 1);

    //绘制圆弧
    CGContextDrawPath(context, kCGPathStroke);
}
```



椭圆

```
- (void)drawRect:(CGRect)rect
{
    CGContextRef circular = UIGraphicsGetCurrentContext();
    CGContextAddEllipseInRect(circular, CGRectMake(100, 100, 100, 80));//宽高相等=圆，宽高不等=椭圆
    CGContextSetRGBFillColor(circular, 1.0, 0, 1.0, 1);// 设置颜色
    CGContextFillPath(circular); // 渲染，将实心圆形画出
}
```



**扇形**

```swift
-(void)drawPie
{
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    CGPoint center = CGPointMake(self.bounds.size.width / 2, self.bounds.size.height / 2);//设置圆心位置
    CGFloat radius = self.bounds.size.height / 2;//设置半径

    CGFloat startAngle = - M_PI_2;//圆起点位置
    CGFloat endAngle = - M_PI_2 +  M_PI * 1.5;//圆终点位置
    [[UIColor greenColor]set];

    //顺时针画扇形
    CGContextMoveToPoint(ctx, center.x, center.y);
    CGContextAddArc(ctx, center.x, center.y, radius, startAngle,endAngle, 0);
    CGContextClosePath(ctx);
    CGContextDrawPath(ctx, kCGPathEOFillStroke);
}
```



#### 绘制文本内容

```swift
//绘图只能在当前位方法中调用,否则无法得到图形上下文
- (void)drawRect:(CGRect)rect{

    //要显示的文字
    NSString *str = @"这是要现实的文本Text。。。。";
    //绘制文字显示的指定区域
    CGRect rect = CGRectMake(20, 50, 374, 500);
    //字体大小
    UIFont *font = [UIFont systemFontOfSize:25];
    //字体颜色
    UIColor *color = [UIColor redColor];
    //初始化段落样式
    NSMutableParagraphStyle *paragraphStyle = [[NSMutableParagraphStyle alloc]init];
    //居中对齐
    NSTextAlignment textAlignment = NSTextAlignmentCenter;
    //设置段落样式
    paragraphStyle.alignment = textAlignment;
    NSDictionary *attributes= @{NSFontAttributeName:font,NSForegroundColorAttributeName:color,
NSParagraphStyleAttributeName:style};
    [str drawInRect:rect withAttributes:attributes];
}
```



绘制图片&&图片剪切`CGContextClip`

```swift
- (void)drawImage{
     UIImage *image = [UIImage imageNamed:@"theImage.jpg"];
    //从某一点开始绘制
    [image drawAtPoint:CGPointMake(10, 50)];
    //绘制到指定的矩形中，注意如果大小不合适会会进行拉伸，图像会形变
    [image drawInRect:CGRectMake(10, 50, 300, 450)];
    //平铺绘制
//    [image drawAsPatternInRect:CGRectMake(0, 0, 320, 568)];
}

//绘图只能在当前位方法中调用,否则无法得到图形上下文
- (void)drawRect:(CGRect)rect{
    //获取图形上下文
    CGContextRef context = UIGraphicsGetCurrentContext();
    //指定上下文显示的范围
    CGContextAddEllipseInRect(context, CGRectMake(80, 50, 100, 100));
    //裁剪
    CGContextClip(context);
    //获取图片
    UIImage *image = [UIImage imageNamed:@"mv2.jpg"];
    [image drawAtPoint:CGPointMake(50, 50)];
}
```


**截屏**

```
//截屏
-(void)cutScreen{
    /*
     1、获得一个图片的画布
    2、获得画布的上下文
    3、设置截图的参数
    4、截图
    5、关闭图片的上下文
    6、保存
    */
   //1、获得一个图片的画布
   UIGraphicsBeginImageContext(self.frame.size);
   //2、获得一个上下文
   [self addRound];
   //3、设置参数
   /*
    CGSize size:截图尺寸
    BOOL opaque：是否不透明,YES不透明
    CGFloat scale：比例
    */
   UIGraphicsBeginImageContextWithOptions(self.frame.size, YES, 1);
   //4、开始截图
   UIImage image = UIGraphicsGetImageFromCurrentImageContext();
   //5、关闭图片上下文
   UIGraphicsEndImageContext();
   //保存到相册
   UIImageWriteToSavedPhotosAlbum(image, self, @selector(image:didFinishSavingWithError:contextInfo:),nil);
}；
```



**保存到相册时的回调方法**

```
- (void)image:(UIImage )image didFinishSavingWithError:(NSError )error
   contextInfo:(void )contextInfo{
}
```



`参考资料推荐`

[**Quartz 2D手册**](https://www.gitbook.com/book/wf851128/quartz-2d-shou-ce/details)

[**ios核心动画高级技巧**](http://www.kancloud.cn/manual/ios)

https://github.com/raineryue?tab=repositories