---
title: iOS常数的定义
tags: iOS
categories: iOS
date: 2017-04-20 17:58:12
grammar_cjkRuby: true
---

在开发中很多人喜欢使用宏定义，但宏定义也有很多缺点。

下面看stackoverflow达人们如果解决常量问题。



```
struct CGSize {
   CGFloat width;
   CGFloat height;
};
typedef struct CGSize CGSize;
```

**Fields** width A width value. height A height value.

```
const CGSize CGSizeZero;
```

e.g

```
static const CGSize pageSize = {320, 480};
```





各种方法，很有意思

http://stackoverflow.com/questions/538996/constants-in-objectivec

http://stackoverflow.com/questions/26252233/global-constants-file-in-swift

[how to group a series of string constants?](http://stackoverflow.com/questions/10312874/objectivec-how-to-group-a-series-of-string-constants)

