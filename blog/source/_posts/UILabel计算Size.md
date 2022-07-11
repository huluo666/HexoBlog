---
title: UIlabel文本计算
tags:
categories:
date: 2018-02-28 14:54:18
grammar_cjkRuby: true
---

一、boundingRectWithSize计算

```objectivec
//UILabel分类方法
- (CGSize)sizeWithFont:(UIFont *)font maxSize:(CGSize)maxSize
{
    CGSize retSize =CGSizeZero;
    if ([self respondsToSelector:@selector(boundingRectWithSize:options:attributes:context:)])
    {
        NSDictionary *attributeDic = @{NSFontAttributeName:font};
        NSStringDrawingOptions options = NSStringDrawingUsesLineFragmentOrigin | NSStringDrawingTruncatesLastVisibleLine;
        retSize = [self boundingRectWithSize:CGSizeMake(maxSize.width, maxSize.height)
                                     options:options
                                  attributes:attributeDic
                                     context:nil].size;
    }
    return CGSizeMake(ceil(retSize.width),
                      ceil(retSize.height));
}
```



二、sizeThatFits方式计算

```
//UILabel分类方法
- (CGSize)preferredSizeWithMaxWidth:(CGFloat)maxWidth
{
    CGSize size = [self sizeThatFits:CGSizeMake(maxWidth, MLFLOAT_MAX)];
    size.width = fmin(size.width, maxWidth); //在numberOfLine=1返回值可能会比maxWidth大
    return size;
}
```

如果是富文本的话，比如包含表情等，使用boundingRectWithSize计算误差就比较大了，所以建议使用sizeThatFits方式计算。

如果在UITableviewCell中需要计算cell高度，不方便获取label实例，我们可以使用一个单例Label来计算高度，减少label实例化带来的性能消耗。

```objectivec
#pragma mark --获取Label高度用--
static UILabel * kProtypeLabel() {
    static UILabel *_protypeLabel = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _protypeLabel = [[UILabel alloc]init];
        _protypeLabel.font=[UIFont systemFontOfSize:16.0f];
        _protypeLabel.numberOfLines = 0;
        _protypeLabel.lineBreakMode=NSLineBreakByCharWrapping;
        //_protypeLabel.textInsets = UIEdgeInsetsMake(5, 5, 5,5);
    });
    return _protypeLabel;
}
```

获取size

```objectivec
#pragma mark --计算富文本高度
#define KMaxWidth 200
+(CGSize)labelSizeWithText:(NSString *)text;
{
    UILabel *label = kProtypeLabel();
    label.text=text;
    return [label preferredSizeWithMaxWidth:KMaxWidth]; //上下间距
}
```

