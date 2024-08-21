---
title: iOS 生成随机数
date: 2016-04-21 16:44:22
tags: iOS
grammar_cjkRuby: true
---

arc4random()

rand()和random()实际并不是一个真正的伪随机数发生器，在使用之前需要先初始化随机种子，否则每次生成的随机数一样。

arc4random() 是一个真正的伪随机算法，不需要生成随机种子，因为第一次调用的时候就会自动生成。而且范围是rand()的两倍。在iPhone中，RAND_MAX是0x7fffffff (2147483647)，而arc4random()返回的最大值则是 0x100000000 (4294967296)。

精确度比较：arc4random()  >  random()  >  rand()。

- 获取一个随机整数范围在：[0,100)包括0，不包括100

```
// 方法1
int x = arc4random() % 100;
// 方法2
int x = arc4random_uniform(100);
```

- 获取一个随机数范围在：[500,1000]，包括500，包括1000

```
// 方法1
int y = (arc4random() % 501) + 500;
// 方法2
int y = arc4random_uniform(500 + 1) + 500;

```

- 获取一个随机整数，范围在[from,to]，包括from，包括to

```
// 范围在[from,to]
- (int)getRandomNumber:(int)from to:(int)to
{
    return arc4random_uniform(to - from + 1) + from;
//    return (arc4random() % (to - from + 1)) + from;
}
```

来源：https://xiaohu860.gitbooks.io/ios/content/xijie_1.html

```objectivec
生成0-x之间的随机正整数
int value =arc4random_uniform(x ＋ 1);

生成随机正整数
int value = arc4random();

通过arc4random() 获取0到x-1之间的整数的代码如下：
int value = arc4random() % x; 
 
获取1到x之间的整数的代码如下: 
int value = (arc4random() % x) + 1; 
 
最后如果想生成一个浮点数，可以在项目中定义如下宏：
#define ARC4RANDOM_MAX      0x100000000 
 
然后就可以使用arc4random() 来获取0到100之间浮点数了（精度是rand()的两倍），代码如下：
double val = floorf(((double)arc4random() / ARC4RANDOM_MAX) * 100.0f);
```



随机色

```
#define RGBColor(r, g, b) [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:1]

#define RGBAColor(r, g, b ,a) [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:a]

//随机色
#define RandColor RGBColor(arc4random_uniform(255), arc4random_uniform(255), arc4random_uniform(255))
```



参考：http://nshipster.cn/random/

http://iphonedevelopment.blogspot.com/2008/10/random-thoughts-rand-vs-arc4random.html