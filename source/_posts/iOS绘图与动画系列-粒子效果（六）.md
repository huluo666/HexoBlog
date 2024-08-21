---
title: iOS绘图与动画系列 粒子效果（六）
date: 2016-04-12 10:47:30
tags: iOS,动画,绘图
grammar_cjkRuby: true
---

`CAEmitterLayer` 粒子发射器

CAEmitterLayer是一个高性能的粒子引擎，被用来创建实时例子动画如：烟雾，火，雪花，雨，爆炸等等这些效果。

#### 一、粒子发送器图层

粒子的常用属性

- birthRate：每秒发送（出生）粒子的数量

> - emitterMode：发送的样式
>   - kCAEmitterLayerPoints：点
>   - kCAEmitterLayerOutline：线
>   - kCAEmitterLayerSurface：面
>   - kCAEmitterLayerVolume：团
> - emitterShape：发送形状的样式
>   - kCAEmitterLayerPoint：点
>   - kCAEmitterLayerLine：线
>   - kCAEmitterLayerRectangle：矩形
>   - kCAEmitterLayerCuboid：立方体
>   - kCAEmitterLayerCircle：曲线
>   - kCAEmitterLayerSphere：圆形
> - renderMode：粒子出现的样式
>   - kCAEmitterLayerOldestFirst：最后出生的粒子，在第一个
>   - kCAEmitterLayerOldestLast：最后出生的粒子，在最后面
>   - kCAEmitterLayerBackToFront：把后面的粒子放到上面
>   - kCAEmitterLayerAdditive：叠加

- emitterCells：在粒子发送器上面添加粒子



`CAEmitterCell：粒子`

> contents：粒子的内容
>
> lifetime：存活的时间
>
> lifetimeRange：存活时间的范围
>
> birthRate：每秒粒子生成的数量
>
> emissionLatitude：散发的纬度（方向：上下）->弧度
>
> emissionLongitude：散发的经度（方向：左右）->弧度
>
> emissionRange：散发的范围 -> 弧度
>
> velocity：发送的速度（速度越快，跑的越远）
>
> velocityRange：发送速度的范围
>
> xAcceleration：X轴的加速度
>
> yAcceleration：Y轴的加速度
>
> zAcceleration：Z轴的加速度
>
> name：粒子的名字。可以通过名字，找到粒子













下雪

```objectivec
//雪花动画
- (void)animation1 {

    //粒子发射器
    CAEmitterLayer *snowEmitter = [CAEmitterLayer layer];
    //粒子发射的位置
    snowEmitter.emitterPosition = CGPointMake(100, 30);
    //发射源的大小
    snowEmitter.emitterSize        = CGSizeMake(self.view.bounds.size.width, 0.0);;
    //发射模式
    snowEmitter.emitterMode        = kCAEmitterLayerOutline;
    //发射源的形状
    snowEmitter.emitterShape    = kCAEmitterLayerLine;

    //创建雪花粒子
    CAEmitterCell *snowflake = [CAEmitterCell emitterCell];
    //粒子的名称
    snowflake.name = @"snow";
    //粒子参数的速度乘数因子。越大出现的越快
    snowflake.birthRate        = 1.0;
    //存活时间
    snowflake.lifetime        = 120.0;
    //粒子速度
    snowflake.velocity        = -10;                // falling down slowly
    //粒子速度范围
    snowflake.velocityRange = 10;
    //粒子y方向的加速度分量
    snowflake.yAcceleration = 2;
      //周围发射角度
    snowflake.emissionRange = 0.5 * M_PI;        // some variation in angle
    //子旋转角度范围
    snowflake.spinRange        = 0.25 * M_PI;        // slow spin
    //粒子图片
    snowflake.contents        = (id) [[UIImage imageNamed:@"DazFlake"] CGImage];
    //粒子颜色
    snowflake.color            = [[UIColor redColor] CGColor];

    //设置阴影
    snowEmitter.shadowOpacity = 1.0;
    snowEmitter.shadowRadius  = 0.0;
    snowEmitter.shadowOffset  = CGSizeMake(0.0, 1.0);
    snowEmitter.shadowColor   = [[UIColor whiteColor] CGColor];

    // 将粒子添加到粒子发射器上
    snowEmitter.emitterCells = [NSArray arrayWithObject:snowflake];
    [self.view.layer insertSublayer:snowEmitter atIndex:0];
}
```



http://www.jianshu.com/p/4b6d60755dd3

http://www.jianshu.com/p/6f5d7cfdae2f

http://www.jianshu.com/p/6a47f7348ed4

