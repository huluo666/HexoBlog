---
title: iOS绘图与动画系列 CAShapeLayer（三）
date: 2016-04-08 15:02:35
tags: iOS,动画,绘图
grammar_cjkRuby: true
---

`CAShapeLayer`是一个通过矢量图形而不是bitmap来绘制的图层子类。你指定诸如颜色和线宽等属性，用`CGPath`来定义想要绘制的图形，最后`CAShapeLayer`就自动渲染出来了。当然，你也可以用Core Graphics直接向原始的`CALyer`的内容中绘制一个路径，相比直下，使用`CAShapeLayer`有`以下一些优点`：

- 渲染快速。`CAShapeLayer`使用了`硬件加速`，绘制同一图形会比用Core Graphics快很多。

- 高效使用内存。一个`CAShapeLayer`不需要像普通`CALayer`一样创建一个寄宿图形，所以无论有多大，都不会占用太多的内存。

- 不会被图层边界剪裁掉。一个`CAShapeLayer`可以在边界之外绘制。你的图层路径不会像在使用Core Graphics的普通`CALayer`一样被剪裁掉（如我们在第二章所见）。

- 不会出现像素化。当你给`CAShapeLayer`做3D变换时，它不像一个有寄宿图的普通图层一样变得像素化。

  来源：[**ios核心动画高级技巧**](http://www.kancloud.cn/manual/ios/97790)



**为什么优先选择CAShapeLayer绘图？**

> CoreGraphics不就可以绘制我们需要的图形，为什么还需要CAShapeLayer来绘图，这是由于UIView的drawRect是用的CoreGraphics框架,是由Cpu进行动画渲染,性能消耗大,而CAShapeLayer属于CoreAnimation框架,其实走的是Gpu动画渲染,使用硬件加速,节省性能，速度也快的多。

基于CAShapeLayer以上优点，以及之前所说的UIBezierPath路径绘制的优势，结合2者用它来生成我们想要的各种图形是最合适不过了。

1. CAShapeLayer是继承至CALayer,可以使用CALayer的所有属性。
2. CAShapeLayer有一个属性path，类型为CGPathRef，而UIBezierPath就是对CGPathRef类型的封装，因此，这两者配合起来使用才可以！

**相关属性**

```markdown
path：不像大多数的动画属性，path不支持隐式动画。
fillColor：填充path的颜色，或无填充。默认为不透明黑色。动画的。
fillRule：填充path的规则。选项是非零和偶奇。默认为非零。
      NSString *const kCAFillRuleNoneZero;
      NSString *const kCAFillRuleEvenOdd;
lineCap：线端点类型
      NSString *const kCALineCapButt; //默认 顶格
      NSString *const kCALineCapRound;//圆角
      NSString *const kCALineCapSquare;//方角
lineDashPattern：线性模版，这是一个NSNumber的数组，索引从1开始记，奇数位数值表示实线长度，偶数位数值表示空白长。
lineDashPhase：线型模版的起始位置。
lineJoin：线连接类型。
       NSString *const kCALineJoinMiter;//默认 尖角
       NSString *const kCALineJoinRound;//圆滑
       NSString *const kCALineJoinBevel;//切割平行
lineWidth：线宽，用点表示单位。可动画。
miterLimit：最大斜接长度。斜接长度指的是在两条线交汇处和外交之间的距离。只有lineJoin属性为kCALineJoinMiter时miterLimit才有效。边角的角度越小，斜接长度就会越大。为了避免斜接长度过长，我们可以使用miterLimit属性。如果斜接长度超过miterLimit的值，边角会以lineJoin的“bevel”即kCALineJoinBevel类型来显示。

strokeColor：用来设置形状的线的颜色，可动画。如果设置为nil就表示没有颜色。
strokeStart和strokeEnd：部分绘线。都是0.0～1.0的取值范围。经常被用来制作动画效果。
```
[理解SVG的图形填充规则](http://blog.csdn.net/cuixiping/article/details/7848369)
 



**绘制步骤**

```swift
- (void)drawRect:(CGRect)rect {
    // Drawing code
    //1、创建BezierPath对象（有类方法与实例方法，具体查看Api）
    UIBezierPath *path = [UIBezierPath bezierPathWithRect:CGRectMake(20, 20, 100, 100)];
    
    //2、创建CAShapeLayer对象并设置相关属性
    CAShapeLayer *shapeLayer = [CAShapeLayer layer];
    shapeLayer.strokeColor = [UIColor redColor].CGColor;
    shapeLayer.fillColor = [UIColor clearColor].CGColor;
    shapeLayer.lineWidth = 5;
    shapeLayer.lineJoin = kCALineJoinRound;
    shapeLayer.lineCap = kCALineCapRound;
    
    
    //3、将贝塞尔路径UIBezierPath（OC）转换为C语言CGPathRef赋值给shapeLayer.path
    shapeLayer.path = path.CGPath;
    
    //4、添加到相应layer层中显示
    [self.layer addSublayer:shapeLayer];
}
```



**画圆**

```swift
- (void)drawRect:(CGRect)rect {
	    //1、创建BezierPath对象
       UIBezierPath  *circleBezierPath = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(50, 350, 100, 100)];//在区域内绘制椭圆曲线

        //2、创建CAShapeLayer对象并设置相关属性
        CAShapeLayer  *circleShapeLayer = [CAShapeLayer layer];
        [circleBezierPath setLineWidth:1];
        [circleShapeLayer setStrokeColor:[UIColor redColor].CGColor];
        [circleShapeLayer setFillColor:[UIColor blackColor].CGColor];
        [circleShapeLayer setBorderWidth:1];
    
        //3、将贝塞尔路径UIBezierPath（OC）转换为C语言CGPathRef赋值给shapeLayer.path
        [circleShapeLayer setPath:circleBezierPath.CGPath];
        [self.layer addSublayer:circleShapeLayer];
 }
```









