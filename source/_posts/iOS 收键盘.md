---
title: iOS隐藏键盘总结
date: 2016-04-11 14:36:24
tags: iOS
grammar_cjkRuby: true
---

`弹出键盘`
`[textField becomeFirstResponder];`


`隐藏键盘`
`resignFirstResponder`、`endEditing`


**方式一.点击Return的时候隐藏键盘（需设置TextField的delegate）**
```
- (BOOL)textFieldShouldReturn:(UITextField *)textField
{
    [textField resignFirstResponder];//需指定文本框的代理 textField.delegate = self;
    return YES;
}
```


**方式二.点击view其他区域隐藏键盘**

```objectivec
 (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    //1.直接交出CSTextField的第一响应者的身份
    [CSTextField resignFirstResponder];


    //2.遍历所有子视图  查找UITextField控件并通知文本失去第一响应者状态
    for (UIView *subVie in self.view.subviews) {
        if ([subVie  isKindOfClass:[UITextField  class]]) {
            [subVie  resignFirstResponder];
        }
    }

    //3.view结束编辑
    [self.view endEditing:YES];


    //4.keyWindow 结束编辑
    [[[UIApplication sharedApplication] keyWindow] endEditing:YES];

    //5.发送resignFirstResponder.
    [[UIApplication sharedApplication] sendAction:@selector(resignFirstResponder) to:nil from:nil forEvent:nil];

    //6.设置textField的Tag
    [[self.view viewWithTag:10001] resignFirstResponder];
}
```



```objectivec
// Scroll 滑动隐藏
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
{
    [self.view endEditing:YES];
}
```



#### 关于UITextView隐藏键盘

**思路基本和UITextField一样**

1.方法一

```objectivec
 //1.（结束编辑）
 [self.view endEditing:YES];
 OR
 [self.text endEditing:YES];

 //2.（注销第一响应）
 [self.text resignFirstResponder];
```

 
2.方法二 （Return键隐藏）

```objectivec
/**
 *  需设置textView的delegate
 */
-(BOOL)textView:(UITextView *)textView shouldChangeTextInRange:(NSRange)range replacementText:(NSString *)text
{
    if ([text isEqualToString:@"\n"]) {
        [textView resignFirstResponder];
        return NO;
    }
    return YES;
}
```


####  **关于 UITableView如何隐藏键盘方法**

```
- (void)viewDidLoad
{
    ...
    UITapGestureRecognizer *gestureRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(hideKeyboard:)];

    gestureRecognizer.cancelsTouchesInView = NO;
    [self.tableView addGestureRecognizer:gestureRecognizer];
}

- (void)hideKeyboard:(UIGestureRecognizer *)gestureRecognizer
{
    CGPoint point = [gestureRecognizer locationInView:self.tableView];
    NSIndexPath *indexPath = [self.tableView indexPathForRowAtPoint:point];

    // Let say you are editing first section first row
    if (indexPath != nil && indexPath.section == 0 && indexPath.row == 0) {
        return;
    }
    [self.firstRowTextField resignFirstResponder];
}
```



Stackflow[Dismiss keyboard by touching background of UITableView](http://stackoverflow.com/questions/2321038/dismiss-keyboard-by-touching-background-of-uitableview)

```objectivec
- (BOOL)findAndResignFirstResonder:(UIView *)stView {
    if (stView.isFirstResponder) {
        [stView resignFirstResponder];
        return YES;
    }
    for (UIView *subView in stView.subviews) {
        if ([self findAndResignFirstResonder:subView]) {
            return YES;
        }
    }
    return NO;
}

- (void)tableView:(UITableView *)tableView
                             didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    ...
    [self findAndResignFirstResonder: self.view];
    ...
}

```