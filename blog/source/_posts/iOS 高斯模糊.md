---
title: iOS 高斯模糊
tags: iOS
categories: iOS
date: 2017-04-13 10:04:51
grammar_cjkRuby: true
---

```objectivec
dispatch_async(dispatch_get_global_queue(0, 0), ^{
        CIContext *context = [CIContext contextWithOptions:nil];
        CIImage *ciImage = [CIImage imageWithCGImage:image.CGImage];
        CIFilter *filter = [CIFilter filterWithName:@"CIGaussianBlur"];
        [filter setValue:ciImage forKey:kCIInputImageKey];
        //设置模糊程度
        [filter setValue:@30.0f forKey: @"inputRadius"];
        CIImage *result = [filter valueForKey:kCIOutputImageKey];
        CGRect frame = [ciImage extent];
        NSLog(@"%f,%f,%f,%f",frame.origin.x,frame.origin.y,frame.size.width,frame.size.height);
        CGImageRef outImage = [context createCGImage: result fromRect:ciImage.extent];
        UIImage * blurImage = [UIImage imageWithCGImage:outImage];
        dispatch_async(dispatch_get_main_queue(), ^{
            coreImgv.image = blurImage;
        });
    });
```

http://www.jianshu.com/p/341a06dd0b46