---
title: iOS  模糊效果实现方式
date: 2016-08-15 18:24:12
tags: iOS
categories: [iOS]
grammar_cjkRuby: true
---



#### 4种方式实现iOS模糊效果

- `CoreImage`中的模糊滤镜

- `UIImage + ImageEffects`的category模糊效果

- iOS8中`UIVisualEffectView`模糊效果

- iOS7以后通过`UIToolBar`实现模糊效果

  http://blog.wangruofeng007.com/blog/2016/01/31/3chong-fang-shi-shi-xian-iosmo-hu-xiao-guo/

http://www.jianshu.com/p/70d3af876909



//透明模糊

- (UIImage *)applyLightEffect;
- 白色模糊

- (UIImage *)applyExtraLightEffect;
- 黑色模糊

- (UIImage *)applyDarkEffect;
- 指定颜色模糊

- (UIImage *)applyTintEffectWithColor:(UIColor *)tintColor;
```

/**
 *  默认透明模糊  不用这个方法了
 */
- (void)refreshBlurViewForNewImage
{
    // 得到截屏
    UIImage *screenShot = [self screenShotOfView:self];
    /**
     *  模糊的速度  模糊层的最终透明度  --默认
     */
    //    screenShot = [screenShot applyBlurWithRadius:30 tintColor:[UIColor colorWithWhite:0.6 alpha:0.2] saturationDeltaFactor:1.0 maskImage:nil];
    screenShot = [screenShot applyLightEffect];
    // 虚化层设置图片
    self.blurryImgView.image = screenShot;
}
```





