---
title: iOS常用计算总结
date: 2016-05-05 18:18:41
tags: iOS
grammar_cjkRuby: true
---

计算如网易，今日头条等新闻栏目选项卡点击按钮后的偏移量

```objectivec
static inline  CGPoint countContentOffset(UIButton *sender,UIScrollView *scrollView) {
    UIButton *currentBtn = sender;
    CGRect  rect = currentBtn.frame;
    CGFloat midX = CGRectGetMidX(rect);
    CGFloat offset = 0;
    CGFloat contentWidth = scrollView.contentSize.width;
    CGFloat halfWidth = CGRectGetWidth(scrollView.bounds) / 2.0;
    //在最左和最右时，标签没必要滚动到中间位置。
    if (midX < halfWidth) {
        offset = 0;
    } else if (midX > contentWidth - halfWidth) {
        offset = contentWidth - 2 * halfWidth;
    } else {
        offset = midX - halfWidth;
    }
    CGPoint contentOffset=CGPointMake(offset, 0);
    return contentOffset;
}
```

**当前scrollview索引**

```
 NSUInteger index = scrollView.contentOffset.x / scrollView.width;
```

获取选中按钮和取消选择中按钮

```objectivec
CGFloat value = ABS(pageView.scrollView.contentOffset.x / pageView.scrollView.frame.size.width);

NSUInteger oneIndex = (NSUInteger)value;
NSUInteger twoIndex = oneIndex + 1;

CGFloat twoPercent = value - oneIndex;
CGFloat onePercent = 1 - twoPercent;
//selectButton
UIButton *oldButton = (UIButton *)[self.scrollView viewWithTag:oneIndex + 1];
UIButton *newButton = (UIButton *)[self.scrollView viewWithTag:twoIndex + 1];
```


#### 颜色过渡效果

```
/**
 *  根据scrollview.contentoffet偏移量改变RGB颜色
 */
static inline  UIColor* ChangeRGBColor(UIColor *fromColor,UIColor *toColor,CGFloat percentage) {
    // get the RGBA values from the colours
    CGFloat fromRed, fromGreen, fromBlue, fromAlpha;
    [fromColor getRed:&fromRed green:&fromGreen blue:&fromBlue alpha:&fromAlpha];

    CGFloat toRed, toGreen, toBlue, toAlpha;
    [toColor getRed:&toRed green:&toGreen blue:&toBlue alpha:&toAlpha];

    //calculate the actual RGBA values of the fade colour
    CGFloat red = (toRed - fromRed) * percentage + fromRed;
    CGFloat green = (toGreen - fromGreen) * percentage + fromGreen;
    CGFloat blue = (toBlue - fromBlue) * percentage + fromBlue;
    CGFloat alpha = (toAlpha - fromAlpha) * percentage + fromAlpha;

    // return the fade colour
    return [UIColor colorWithRed:red green:green blue:blue alpha:alpha];
}
```


tableView不顶格

```
if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 7) {
    self.edgesForExtendedLayout = UIRectEdgeNone;
    self.extendedLayoutIncludesOpaqueBars = NO;
    self.automaticallyAdjustsScrollViewInsets = NO;
}
```

