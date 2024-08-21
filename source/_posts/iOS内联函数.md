---
title: iOS内联函数
date: 2017-12-29 09:33:45
tags:
categorys:
---



解决函数调用效率

- 函数之间调用，是内存地址之间的调用，当函数调用完毕之后还会返回原来函数执行的地址。函数调用有时间开销，内联函数就是为了解决这一问题。

  ​

使用

**AFNetWorking**

```objectivec
static inline NSString * AFContentTypeForPathExtension(NSString *extension) {
    NSString *UTI = (__bridge_transfer NSString *)UTTypeCreatePreferredIdentifierForTag(kUTTagClassFilenameExtension, (__bridge CFStringRef)extension, NULL);
    NSString *contentType = (__bridge_transfer NSString *)UTTypeCopyPreferredTagWithClass((__bridge CFStringRef)UTI, kUTTagClassMIMEType);
    if (!contentType) {
        return @"application/octet-stream";
    } else {
        return contentType;
    }
}
```



```
static inline CGSize Label_CGSizePixel(CGSize size) {
    static CGFloat scale = 0.0f;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        scale = [UIScreen mainScreen].scale;
    });
    return CGSizeMake(round(size.width * scale) / scale,
                      round(size.height * scale) / scale);
}


CG_INLINE void POST_NOTIFICATION(NSString *name, id object, NSDictionary *userInfo){
    [[NSNotificationCenter defaultCenter] postNotificationName:name object:object userInfo:userInfo];
}

#  define CG_INLINE static inline
```

示例

```objectivec
//.h
#import <UIKit/UIKit.h>
FOUNDATION_EXPORT inline NSString * myInlineFunction(void);

static inline void method(int a,char *b) {
    //todo
}

//或者
extern Type Example(void);
inline Type Example(void)
{
    //..........
}
```

