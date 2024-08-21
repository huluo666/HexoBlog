---
title: UITableViewHeaderFooterView复用
date: 2016-05-29 16:38:42
tags: iOS
categories: [iOS]
grammar_cjkRuby: true
---

1.首先要自定义一个sectionHeadView／sectionFootView继承自 UITableViewHeaderFooterView，如下：

```objectivec
#import <UIKit/UIKit.h>

@interface CircleHeaderFooterView : UITableViewHeaderFooterView

@end
```

2.在自定义的sectionHeadView／sectionFootView中重写这个方法，设置复用

```objectivec
#import "CircleHeaderFooterView.h"

@implementation CircleHeaderFooterView

-(instancetype)initWithReuseIdentifier:(NSString *)reuseIdentifier
{
    self = [super initWithReuseIdentifier:reuseIdentifier];
    if (self) {
        //表示初始化方法
    }
    return self;
}
@end
```



3.在需要调用自定义sectionHeadView／sectionFootView的VC里面调用table的代理方法，用法跟cell的复用相似

```objectivec
- (nullable UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section{
    static NSString *viewIdentfier = @"headView";
    CircleHeaderFooterView *sectionHeadView = [tableViewdequeueReusableHeaderFooterViewWithIdentifier:viewIdentfier];
    if(!sectionHeadView){
        sectionHeadView = [[CircleHeaderFooterView alloc]initWithReuseIdentifier:viewIdentfier];
        sectionHeadView.contentView.backgroundColor = [UIColor whiteColor];
    }
    return sectionHeadView;
}

```



新写法

```objective c
1：注册headerView
[self.tableView registerClass:[CircleHeaderFooterView class] forHeaderFooterViewReuseIdentifier:@"section"];

2：使用
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
{
    CircleHeaderFooterView *headView = [tableView dequeueReusableHeaderFooterViewWithIdentifier:@"section"];
    headView.textLabel.text = [NSString stringWithFormat:@"section %ld", section];
    return headView;
}
```


系统自带

```objectivec

- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
{
    UITableViewHeaderFooterView *headView = [tableView dequeueReusableHeaderFooterViewWithIdentifier:@"section"];
    if (!headView) {
        headView = [[UITableViewHeaderFooterView alloc] initWithReuseIdentifier:@"section"];
    }
    headView.textLabel.text = [NSString stringWithFormat:@"section %ld", section];
    return headView;
}
```

