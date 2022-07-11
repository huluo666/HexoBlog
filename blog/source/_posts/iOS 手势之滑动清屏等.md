---
title: iOS 手势之滑动清屏等
date: 2016-09-30 11:54:58
tags: iOS
categories: [iOS]
grammar_cjkRuby: true
---

`translationInView`:（获取当前拖拽位置）
`translationInView` 返回父视图系统当中，返回横坐标，纵坐标上拖动了多少像素

可以判断拖动方向

```objectivec
CGPoint translantion = [UIPanGestureRecognizer translationInView:UIView];
ABS(translantion.x)/ABS(translantion.y) > 1
//即判断是否水平移动(x轴拖动距离大于y轴拖动距离)
```

`velocityInView`:（设置拖拽速度，单位：像素/秒）
`velocityInView` 返回指定坐标系统当中拖动的速度，x,y分别代表x轴y轴的拖动速度(矢量)

```objectivec
CGPoint velocity = [UIPanGestureRecognizer velocityInView:UIView];
velocity.x > 0 //向右 x,y正负值 判断上下左右
```

实例应用

```objectivec
// 枚举值，包含水平移动方向和垂直移动方向
typedef NS_ENUM(NSInteger, PanDirection){
    PanDirectionHorizontalMoved, //横向移动
    PanDirectionVerticalMoved    //纵向移动
};

static PanDirection panDirection;
- (void)panGestureOnMainVC2:(UIPanGestureRecognizer *)sender{
    if (sender.numberOfTouches == 0) {
        NSLog(@"手指离开屏幕");
    }
    //根据在view上Pan的位置，确定是跳音量、亮度
    // CGPoint locationPoint = [pan locationInView:self];
    // 我们要响应水平移动和垂直移动
    // 根据上次和本次移动的位置，算出一个速率的point
    CGPoint veloctyPoint = [sender velocityInView:self.view];
    CGPoint transPoint = [sender translationInView:self.view];
    switch (sender.state) {
        case UIGestureRecognizerStateBegan:{
            NSLog(@"x:%f  y:%f   aaa:%f,bbb:%f",veloctyPoint.x, veloctyPoint.y,transPoint.x,transPoint.y);
            // 使用绝对值来判断移动的方向
            CGFloat x = fabs(veloctyPoint.x);
            CGFloat y = fabs(veloctyPoint.y);
            if (x > y) { // 水平移动
                panDirection = PanDirectionHorizontalMoved;
            }else if (x < y){ // 垂直移动
                panDirection = PanDirectionVerticalMoved;
            }
        }
            break;
        case UIGestureRecognizerStateChanged:{
            switch (panDirection) {
                case PanDirectionHorizontalMoved:{

                    [self horizontalMoved:veloctyPoint.x]; // 水平移动的方法只要x方向的值
                    break;
                }
                case PanDirectionVerticalMoved:{
                    [self verticalMoved:veloctyPoint.y]; // 垂直移动方法只要y方向的值
                    break;
                }
                default:
                    break;
            }

        }
            break;
        case UIGestureRecognizerStateEnded:{
            // 移动结束也需要判断垂直或者平移
            // 比如水平移动结束时，要快进到指定位置，如果这里没有判断，当我们调节音量完之后，会出现屏幕跳动的bug
            NSLog(@"UIGestureRecognizerStateEnded");
            switch (panDirection) {
                case PanDirectionHorizontalMoved:{//快进结束

                }
                    break;
                case PanDirectionVerticalMoved:{ //垂直移动结束后，隐藏音量控件 且，把状态改为不再控制音量

                }
                    break;

                default:
                    break;
            }
        }
            break;

        default:
            break;
    }
}
```


设置最小滑动距离，相比用屏幕宽度*0.5为或滑动方向为标准判断是否清屏体验要好。

```objectivec
UIView *bottomView;
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    self.view.backgroundColor=[UIColor whiteColor];

    UIView *panview =[[UIView alloc]initWithFrame:self.view.bounds];
    panview.backgroundColor=[UIColor orangeColor];
    [self.view addSubview:panview];
    bottomView=panview;
    [self.view addGestureRecognizer:[[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(panGestureOnMainVC:)]];
  }
```

```objectivec
#pragma mark---滑动清屏幕功能
//最小滑动间距
static CGFloat const MIN_SPACE_VALUE= 50.0;
-(void)panGestureOnMainVC:(UIPanGestureRecognizer *)sender
{
    //  获取平移值(//这个是获取手指从初始位置移动的偏移量)
    CGPoint translation = [sender translationInView:self.view];
    if (sender.state == UIGestureRecognizerStateBegan) {
    }

    UIView *panView= bottomView;
    panView.transform = CGAffineTransformTranslate(panView.transform, translation.x, 0);

    // 清零移动叠加
    //再把形变设置为0，不然会因为不断调用这个方法，值会不断叠加，，
    //例如从0 - 3的距离应该只移动3，但是却变1+2+3 = 6，移动了6的距离
    [sender setTranslation:CGPointZero inView:self.view];


    //获取右边极限位置
    CGAffineTransform  rightScopeTransform=CGAffineTransformMakeTranslation(self.view.frame.size.width, 0);
    //右边极限位置
    if (panView.transform.tx > rightScopeTransform.tx) {
        panView.transform = rightScopeTransform;
    }else if (panView.transform.tx < 0.0){
    //左边极限位置
        panView.transform=CGAffineTransformMakeTranslation(0, 0);
    }

    CGPoint veloctyPoint = [sender velocityInView:self.view];
    //    当托拽手势结束时执行
    if (sender.state == UIGestureRecognizerStateEnded)
    {
        [UIView animateWithDuration:0.2 delay:0 options:UIViewAnimationOptionCurveEaseIn animations:^{
            if (veloctyPoint.x>0) {//向右
                if (panView.transform.tx<MIN_SPACE_VALUE) {
                    panView.transform = CGAffineTransformIdentity;
                }else{
                    panView.transform=rightScopeTransform;
                }
            }else{//向左
                if (panView.transform.tx>(self.view.frame.size.width-MIN_SPACE_VALUE)) {
                    panView.transform=rightScopeTransform;
                }else{
                    panView.transform = CGAffineTransformIdentity;
                }
            }
        } completion:^(BOOL finished) {

        }];
    }
}
```