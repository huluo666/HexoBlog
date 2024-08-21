---
title: iOS 中的系统常用宏定义NSObjCRuntime.h
date: 2016-04-15 18:17:48
tags: iOS
grammar_cjkRuby: true
---



下列宏定义查看API：NSObjCRuntime.h,CGBase.h

当我们在 objectivec 中引入了 `Foundation Framework` 之后，最好是使用系统宏定义，比如用`FOUNDATION_EXPORT` 来代替 `extern`, 因为在 `NSObjCRuntime.h` 中它包括了一些 C 和 C++ 的库，为了能更好的和这些 C 和 C++ 库兼容，所以建议用 `FOUNDATION_EXPORT`。



`内联函数` （`NS_INLINE`）

```
NS_INLINE
//UIKit框架 UIKIT_STATIC_INLINE
static inline CGRect ScaleRect(CGRect rect, float n)
```



`定义常量` （`FOUNDATION_EXPORT & #define`）

```
FOUNDATION_EXPORT NSString *NSStringFromClass(Class aClass);
//UIKit框架 `UIKIT_EXTERN
.h  FOUNDATION_EXPORT NSString * const kMyConstantString;
.m  NSString * const kConstantString = @"Hello";
```

```
#define kConstantString @"Hello"
```

`FOUNDATION_EXPORT`可以直接比较字符串指针地址，效率`#define`比高

UIKit框架 `UIKIT_EXTERN`



取最大值、最小值、绝对值

    #define MIN(A,B)	((A) < (B) ? (A) : (B))
    #define MAX(A,B)	((A) > (B) ? (A) : (B))
    #define ABS(A)		((A) < 0 ? (-(A)) : (A))





iOS常用示例

计算最大值、最小值、绝对值

define MIN(A,B) ((A) < (B) ? (A) : (B))

define MAX(A,B) ((A) > (B) ? (A) : (B))

define ABS(A)  ((A) < 0 ? (-(A)) : (A))



//计算scrollView索引

int index = ABS(scrollView.contentOffset.x) / scrollView.frame.size.width;









 





 `ABS`

```swift
int abs(int i);                  // 处理int类型的取绝对值
double fabs(double i); 			 //处理double类型的取绝对值
float fabsf(float i);            //处理float类型的取绝对值

double a = -2.6;
NSLog(@"%.2f",ABS(a));//2.60
int b = -4.0;
NSLog(@"%d",ABS(b));//4
```



```
  CGFloat maxSide = MAX(fabsf(point1.x - point2.x ), fabsf(point2.y - point2.y));
```

四舍五入

```swift
roundf(<#float#>)
指定精度取整函数: round
语法: round(double a, int d)
返回值: DOUBLE
说明: 返回指定精度d的double类型

举例：
round(3.1415926,4)   //3.1416
```

```swift
//    取整
    double f =2.5;
    NSLog(@"%.0f",round(f));//3

//    向下取整
    NSLog(@"%f",floor(f));//2.000000

//  向上取整
    NSLog(@"%f",ceil(f));
```



rand() ----随机数

abs() / labs() ----整数绝对值

fabs() / fabsf() / fabsl() ----浮点数绝对值

floor() / floorf() / floorl() ----向下取整

ceil() / ceilf() / ceill() ----向上取整

round() / roundf() / roundl() ----四舍五入

sqrt() / sqrtf() / sqrtl() ----求平方根

fmax() / fmaxf() / fmaxl() ----求最大值

fmin() / fminf() / fminl() ----求最小值

hypot() / hypotf() / hypotl() ----求直角三角形斜边的长度

 fmod() / fmodf() / fmodl() ----求两数整除后的余数

modf() / modff() / modfl() ----浮点数分解为整数和小数

frexp() / frexpf() / frexpl() ----浮点数分解尾数和二为底的指数

sin() / sinf() / sinl() ----求正弦值

sinh() / sinhf() / sinhl() ----求双曲正弦值

cos() / cosf() / cosl() ----求余弦值

cosh() / coshf() / coshl() ----求双曲余弦值

tan() / tanf() / tanl() ----求正切值

tanh() / tanhf() / tanhl() ----求双曲正切值

asin() / asinf() / asinl() ----求反正弦值

asinh() / asinhf() / asinhl() ----求反双曲正弦值

acos() / acosf() / acosl() ----求反余弦值

 acosh() / acoshf() / acoshl() ----求反双曲余弦值

atan() / atanf() / atanl() ----求反正切值

atan2() / atan2f() / atan2l() ----求坐标值的反正切值

atanh() / atanhf() / atanhl() ----求反双曲正切值















http://read.pudn.com/downloads61/ebook/211392/%E9%AB%98%E7%BA%A7%E8%AF%AD%E8%A8%80c%2B%2B%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/%E9%99%84%E5%BD%95B.pdf





http://blog.csdn.net/abc649395594/article/details/44730425

https://www.gitbook.com/book/eilianlove/objectivec-guide/details