---
title: iOS判断是present还是push
date: 2017-12-22 13:53:06
tags:
categorys: iOS
---



方式

```objectivec
- (BOOL)isModal {
    return self.presentingViewController.presentedViewController == self
      || (self.navigationController != nil && self.navigationController.presentingViewController.presentedViewController == self.navigationController)
      || [self.tabBarController.presentingViewController isKindOfClass:[UITabBarController class]];
}


- (BOOL)isModal {
     if([self presentingViewController])
         return YES;
     if([[[self navigationController] presentingViewController] presentedViewController] == [self navigationController])
         return YES;
     if([[[self tabBarController] presentingViewController] isKindOfClass:[UITabBarController class]])
         return YES;

    return NO;
 }
```

https://stackoverflow.com/questions/2798653/is-it-possible-to-determine-whether-viewcontroller-is-presented-as-modal







**Push**或**Pop**动画

**For Push** (on `MainViewController`)

```objectivec
LoginViewController *VC = [[LoginViewController alloc]init];
CATransition* transition = [CATransition animation];
transition.duration = 0.3f;
transition.type = kCATransitionMoveIn;
transition.subtype = kCATransitionFromTop;
[self.navigationController.view.layer addAnimation:transition
                                            forKey:kCATransition];
[[[UINavigationController alloc] initWithRootViewController:VC] pushViewController:VC animated:NO];
//[self.navigationController pushViewController:VC animated:NO];
```

**For Pop** (on `LoginViewController`)

```objectivec
CATransition* transition = [CATransition animation];
transition.duration = 0.3f;
transition.type = kCATransitionReveal;
transition.subtype = kCATransitionFromBottom;
[self.navigationController.view.layer addAnimation:transition
                                            forKey:kCATransition];
[self.navigationController popViewControllerAnimated:NO];
```

