---
title: const常量与define宏定义，结构体的区别
tags: iOS
categories: iOS
date: 2017-04-21 17:23:31
grammar_cjkRuby: true
---

#### #define和const区别

**(1) 编译器处理方式不同**

- `编译时刻`:define宏是预编译（编译之前处理），const是编译运行阶段。

**(2) 类型和编译安全检查不同**

- define宏没有类型，不做任何类型检查，仅仅是替换展开，不会报编译错误。
- const常量有具体的类型，在编译阶段会执行类型检查，会报编译错误。
- 有些集成化的调试工具可以对const常量进行调试，但是不能对宏常量进行调试。

**(3) 存储方式不同**

- define宏仅仅是展开，有多少地方使用，就展开多少次，不会分配内存。（宏定义不分配内存，**变量定义**分配内存。）
- const常量会在内存中分配(可以是堆中也可以是栈中)。

**(4)const  可以节省空间，避免不必要的内存分配。**

 例如：

```objectivec
#define PI 3.14159 //常量宏
const double Pi=3.14159; //此时并未将Pi放入ROM中 ......
double i=Pi; //此时为Pi分配内存，以后不再分配！
double I=PI; //编译期间进行宏替换，分配内存
double j=Pi; //没有内存分配
double J=PI; //再进行宏替换，又一次分配内存！
```
宏是直接替换的,它会产生许多个临时的存储空间来存储需要替换的部分,这样会浪费内存,没有必要。 const定义常量从汇编的角度来看，只是给出了对应的内存地址，而不是象#define一样给出的是立即数，所以，const定义的常量在程序运行过程中只有一份拷贝（因为是全局的只读变量，存在静态区），而 #define定义的常量在内存中有若干个拷贝。

**(5) 提高了效率。**

 编译器通常不为普通const常量分配存储空间，而是将它们保存在符号表中，这使得它成为一个编译期间的常量，没有了存储与读内存的操作，使得它的效率也很高。使用大量#define宏，容易造成编译时间久，每次都需要重新替换。

**(6) 宏替换只作替换，不做计算，不做表达式求解;**

宏预编译时就替换了，程序运行时，并不分配内存。



#### static简介

- 修饰局部变量：
  1.延长局部变量的生命周期,程序结束才会销毁。
  2.局部变量只会生成一份内存,只会初始化一次。
- 修饰全局变量
  1.只能在本文件中访问,修改全局变量的作用域,生命周期不会改。



#### const简介

之前常用的字符串常量，一般是抽成宏，但是苹果不推荐我们抽成宏，推荐我们使用const常量。

const在C语言中算是一个比较新的描述符，我们称之为常量修饰符，意即其所修饰
的对象为常量(immutable)。

const只修饰它右边的内容，被const修饰的内容都是常量、都是不能再修改的

```c++
// （基本数据变量p，指针变量*p）
// const修饰指针变量访问的内存空间，修饰的是右边*p1，
int  *const p1; // p1:常量 *p1:变量--(*常量=指针变量)
int  const *p2; // p2:变量 *p2:常量
const int *p3;  // p3:变量 *p3:常量

// 第一个const修饰*p4 第二个const修饰 p4
// 两种方式一样，也即被const修饰的内容都是常量
const int * const p4;  //  p4：常量 *p4：常量
int const * const p5;  //  p5：常量 *p4：常量
```

- 注：判断p和p是只读还是变量 关键是看const在谁前面。
  - 如果只在p前面，那么p只读 *p还是变量；
  - 如果在*p前面，p是变量   *p是只读。

`extern`与`const`联合使用

开发中使用场景:在多个文件中经常使用的同一个字符串常量，可以使用extern与const组合。

原因:

- static与const组合：在每个文件都需要定义一份静态全局变量。
- extern与const组合:只需要定义一份全局变量，多个文件共享。

例如：

AppConst.h

```objectivec
static NSString *const kStr1  = @"kimi";//此处定义的kStr1不能改变，否则会发生错误
static NSString  const *kStr2 = @"kimi";//跟上面的定义写法不同，但是结果一样
static const NSString  *kStr3 = @"kimi";//此处定义的kStr可修改其值，但是修改过后他们的内存地址一样。

extern NSString *const kStr4;
extern NSString *const kStr5;
```

AppConst.m

```objectivec
NSString *const kStr4 = @"kimi";
NSString *const kStr5 = @"kimi";
```



```
2017-04-21 14:47:02.077  kStr1内存地址： 0x109ddd0e0=FirstViewController
2017-04-21 14:47:02.078  kStr2内存地址： 0x109dde408=FirstViewController
2017-04-21 14:47:02.079  kStr3内存地址： 0x109dde410=FirstViewController
2017-04-21 14:47:02.079  kStr4内存地址： 0x109ddd0d0=FirstViewController
2017-04-21 14:47:02.080  kStr5内存地址： 0x109ddd0d8=FirstViewController

2017-04-21 14:47:27.747  kStr1内存地址： 0x109ddd080=SecViewController
2017-04-21 14:47:27.748  kStr2内存地址： 0x109dde320=SecViewController
2017-04-21 14:47:27.748  kStr3内存地址： 0x109dde328=SecViewController
2017-04-21 14:47:27.748  kStr4内存地址： 0x109ddd0d0=SecViewController
2017-04-21 14:47:27.749  kStr5内存地址： 0x109ddd0d8=SecViewController

2017-04-21 14:47:51.019  kStr1内存地址： 0x109ddd088=TestViewController
2017-04-21 14:47:51.019  kStr2内存地址： 0x109dde3f0=TestViewController
2017-04-21 14:47:51.020  kStr3内存地址： 0x109dde3f8=TestViewController
2017-04-21 14:47:51.020  kStr4内存地址： 0x109ddd0d0=TestViewController
2017-04-21 14:47:51.020  kStr5内存地址： 0x109ddd0d8=TestViewController
```

结论：使用`static NSString *const xxx  = @"xxx"`; 申明的静态全局常量，经过测试页面消失进入，相同Controller打印的内存地址相同，在不同Controller中内存地址不同，表明如果在不同文件中使用会占据多个内存空间，而使用 `extern NSString *const xxxx` 申明的全局静态变量，在不同文件中的内存地址都是相同的，表面只开辟一个内存空间。



所以，全局常量正规写法:开发中便于管理所有的全局变量，通常创建GlobeConst文件，里面专门定义全局变量，统一管理。

```objectivec
//GlobeConst.h
extern NSString * const KNameKey = @"kimi";
FOUNDATION_EXPORT NSString *const MyFirstConstant;
FOUNDATION_EXPORT NSString *const MySecondConstant;
//etc.

//GlobeConst.m
NSString * const    KNameKey = @"kimi";
NSString *const MyFirstConstant = @"FirstConstant";
NSString *const MySecondConstant = @"SecondConstant";
```

当然由于开发中常用数据类型所占字节很小，所以使用`static NSString *const xxx  = @"xxx"`来申明全局常量问题也不大，可以不在GlobeConst.m中添加代码，相对方便。

使用字符串常量代替 `#define`宏定义会常数是可以使用指针比较相等性测试 (`stringInstance == MyFirstConstant`) 这比字符串比较快得多 (`[stringInstance isEqualToString:MyFirstConstant]`)

传送门：[常用数据类型对应字节数](http://huluo666.cn/2016/05/23/%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%B8%B8%E9%87%8F/)



##### 其它全局常量的一些写法

```objectivec
// in the header
extern const struct MANotifyingArrayNotificationsStruct
{
    NSString *didAddObject;
    NSString *didChangeObject;
    NSString *didRemoveObject;
} MANotifyingArrayNotifications;

// in the implementation
const struct MANotifyingArrayNotificationsStruct MANotifyingArrayNotifications = {
    .didAddObject = @"didAddObject",
    .didChangeObject = @"didChangeObject",
    .didRemoveObject = @"didRemoveObject"
};

//访问
   NSLog(@"%@",MANotifyingArrayNotifications.didAddObject);
```



定义UIKit常量注意将.m文件改成.mm

```objectivec
+ (UIColor *)paleYellowColor
{
    static UIColor* paleYellow = nil;
    if (paleYellow == nil)
    {
        paleYellow = [UIColor colorWithHueDegrees:60 saturation:0.2 brightness:1.0];
    }
    return paleYellow;
}

.h文件
extern UIColor *  const COLOR_LIGHT_BLUE;

.mm文件
UIColor* const COLOR_LIGHT_BLUE = [[UIColor alloc] initWithRed:21.0f/255 green:180.0f/255  blue:1 alpha:1];
```



使用单例保存全局变量

[+initialize和+load](http://zhangbuhuai.com/initialize-and-load-in-objectivec/)

