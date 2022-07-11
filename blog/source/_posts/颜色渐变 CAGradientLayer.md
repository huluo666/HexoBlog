---
title: 颜色渐变 CAGradientLayer
date: 2016-04-06 14:40:43
tags: iOS,动画
grammar_cjkRuby: true
---
如果要完成需要有渐变色的图片，进度条等，那么`CAGradientLayer`可以很容易的实现这一功能。

点击进入`CAGradientLayer.h`文件
阅读Api
Api简介：
在背景色上绘制渐变色，填满layer（包括圆角）

```objectivec
/**
 *  CAGradientLayer是CALayer的子类
 */
@interface CAGradientLayer : CALayer


/**
 *  用作渐变的颜色数组
 */
@property(nullable, copy) NSArray *colors;

/**
 *  用作分界的位置数组
 */
@property(nullable, copy) NSArray<NSNumber *> *locations;

/**
 *  渐变开始位置
 */
@property CGPoint startPoint;

/**
 *  渐变结束位置
 */
@property CGPoint endPoint;

/**
 *  表示像素变化方式，只有一个值
 */
@property(copy) NSString *type;

@end
```


```objectivec
-(void)viewDidLoad
{
    [super viewDidLoad];
    //TODO: 1、先创建一个label
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(100, 200, 200, 28)];
    label.text = @"颜色渐变hello world";
    [self.view addSubview:label];
    
    //TODO: 2、创建layer（负责显示渐变色）
    CAGradientLayer *layer = [CAGradientLayer layer];//初始化渐变层
    layer.frame = label.bounds;
    
    // 设置颜色组
    layer.colors = @[(__bridge id)[UIColor redColor].CGColor,
                     (__bridge id)[UIColor greenColor].CGColor,
                     (__bridge id)[UIColor blackColor].CGColor,
                     (__bridge id)[UIColor whiteColor].CGColor,
                     (__bridge id)[UIColor blueColor].CGColor
                     ];
    // 颜色分割线,渐变颜色的区间分布，locations的数组长度和color一致，这个值一般不用管它，默认是nil，会平均分布。
    layer.locations = @[@(0.2), @(0.35),@(0.5),@(0.65),@(0.7)];
    // 设置起点和终点坐标
    layer.startPoint = CGPointMake(0, 0);
    layer.endPoint = CGPointMake(1, 1);
    layer.type = kCAGradientLayerAxial;
    layer.bounds = CGRectMake(0, 0, 400, 400);
    layer.position = CGPointMake(200, 340);
    layer.mask = label.layer;
    [self.view.layer addSublayer:layer];
}
```


### 方式二 drawRect颜色渐变
首先创建一个 UIView，重写drawRect方法
- 一 创建颜色空间
- 二 创建渐变
- 三 设置裁剪区域
- 四 绘制渐变
- 五 释放对象


``` objectivec
-(void)drawRect:(CGRect)rect
{
    [self drawGradient];
//    [self drawGradient2];
}

#pragma mark ---线性渐变
-(void) drawGradient
{
    CGContextRef context = UIGraphicsGetCurrentContext();
    //1.创建颜色空间 在计算机设置中，直接选择rgb即可，其他颜色空间暂时不用考虑。
    CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB();
    
    //2.创建渐变
    CGFloat components[8] = {1.0,0.0,0.0,1.0,1.0,1.0,1.0,1.0};
    CGFloat locations[2] = {0.0,1.0};
    
    CGGradientRef gradient=CGGradientCreateWithColorComponents(colorSpace, components, locations, 2);
    //3.渐变区域裁剪
    //CGContextClipToRect(context, CGRectMake(0, 360, 200, 100));
    CGRect rects[5] = {CGRectMake(0, 0, 100, 100),CGRectMake(100, 100, 100, 100),CGRectMake(200, 0, 100, 100),CGRectMake(0, 200, 100, 100),CGRectMake(200, 200, 100, 100)};
    CGContextClipToRects(context,rects,5);
    //4.绘制渐变
    CGContextDrawLinearGradient(context, gradient, CGPointMake(0.0, 0.0), CGPointMake(370, 460.0), kCGGradientDrawsAfterEndLocation);
    //5.释放对象
    CGColorSpaceRelease(colorSpace);
    CGGradientRelease(gradient);
}

#pragma mark 径向渐变
-(void)drawGradient2
{
    CGContextRef context = UIGraphicsGetCurrentContext();
    CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB();
    CGFloat components[8] = {1.0,0.0,0.0,1.0,1.0,1.0,1.0,1.0};
    CGFloat locations[2] = {0.3,1.0};
    CGGradientRef gradient=CGGradientCreateWithColorComponents(colorSpace, components, locations, 2);
    
    CGPoint statCenter=CGPointMake(160, 230);   //起始中心点
    CGFloat startRadius=10;                     //起始半径 指定为0  从圆心渐变  否则  中心位置不渐变
    CGPoint endCenter=CGPointMake(160, 230);    //结束中心点（通常与起始专心点重合）
    CGFloat endRadius=150;                      //结束半径
    CGContextDrawRadialGradient(context, gradient,statCenter,startRadius,endCenter,endRadius,kCGGradientDrawsAfterEndLocation);
    //释放对象
    CGColorSpaceRelease(colorSpace);
    CGGradientRelease(gradient);
}
```