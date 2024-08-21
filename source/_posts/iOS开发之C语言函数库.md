---
title: iOS开发之C语言函数库
date: 2016-04-16 10:17:48
tags: iOS
grammar_cjkRuby: true
---

在iOS开发中不可避免的需要使用数学函数进行计算，我们可以使用C标准库头文件<math.h>中定义的数学常量宏及数学函数来进行基本的数学计算操作，这些C函数都可以在微软的`MSDN`API库中找到。(https://msdn.microsoft.com/zh-cn/library )

**一、 iOS常用示例如：**
```
//计算最大值、最小值、绝对值
#define MAX(A,B) ((A) > (B) ? (A) : (B))
#define MIN(A,B) ((A) < (B) ? (A) : (B))
#define ABS(A) ((A) < 0 ? (-(A)) : (A))

//计算scrollView索引
int index = ABS(scrollView.contentOffset.x) / scrollView.frame.size.width;
```

![](http://upload-images.jianshu.io/upload_images/328273-4e58afd530e75b1d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

相关函数申明请查看头文件`math.h`
**二、具体函数释义如下：**
```
/************************************************************
 *                                                          *
 *                  Math Functions（数学函数）                *
 *                                                          *
 ***********************************************************/

#pragma mark --- 三角函数  a前缀表示取反  asin反三角函数
//TODO: 求反余弦值
extern float acosf(float);
extern double acos(double);
extern long double acosl(long double);

//TODO: 求反正弦值
extern float asinf(float);
extern double asin(double);
extern long double asinl(long double);

//TODO: 求反正切值
extern float atanf(float);
extern double atan(double);
extern long double atanl(long double);

//TODO: 求坐标值的反正切值
extern float atan2f(float, float);
extern double atan2(double, double);
extern long double atan2l(long double, long double);

//TODO: 求余弦值
extern float cosf(float);
extern double cos(double);
extern long double cosl(long double);

//TODO: 求正弦值
extern float sinf(float);
extern double sin(double);
extern long double sinl(long double);

//TODO: 求正切值
extern float tanf(float);
extern double tan(double);
extern long double tanl(long double);

//TODO: 求反双曲余弦值
extern float acoshf(float);
extern double acosh(double);
extern long double acoshl(long double);

//TODO: 求反双曲正弦值
extern float asinhf(float);
extern double asinh(double);
extern long double asinhl(long double);

//TODO: 求反双曲正切值
extern float atanhf(float);
extern double atanh(double);
extern long double atanhl(long double);

//TODO: 求双曲余弦值
extern float coshf(float);
extern double cosh(double);
extern long double coshl(long double);

//TODO: 求双曲正弦值
extern float sinhf(float);
extern double sinh(double);
extern long double sinhl(long double);

//TODO: 求双曲正切值
extern float tanhf(float);
extern double tanh(double);
extern long double tanhl(long double);

//TODO: 求取自然数e的幂
extern float expf(float);
extern double exp(double);
extern long double expl(long double);

//TODO: 计算参数以 2 为底的指数
extern float exp2f(float);
extern double exp2(double);
extern long double exp2l(long double);

//TODO: 计算基础 e 指数参数，但不包括 1
extern float expm1f(float);
extern double expm1(double);
extern long double expm1l(long double);

//TODO: 以e为底的对数（log10f则为10为底、log2f：2为底 ）
extern float logf(float);
extern double log(double);
extern long double logl(long double);

extern float log10f(float);//以10为底的对数
extern double log10(double);
extern long double log10l(long double);

extern float log2f(float);//以2为底的对数
extern double log2(double);
extern long double log2l(long double);

//TODO: 计算基础 e 对数 1 加上参数
extern float log1pf(float);
extern double log1p(double);
extern long double log1pl(long double);

//TODO: 提取 _X 指数，作为一个带符号整数值为浮点格式
extern float logbf(float);
extern double logb(double);
extern long double logbl(long double);

//TODO: 浮点数分解为整数和小数。返回 _X 的有符号的小数部分
extern float modff(float, float *);
extern double modf(double, double *);
extern long double modfl(long double, long double *);

//TODO: 计算从尾数和指数的一个实数
extern float ldexpf(float, int);
extern double ldexp(double, int);
extern long double ldexpl(long double, int);

//TODO: 浮点数分解尾数和二为底的指数
extern float frexpf(float, int *);
extern double frexp(double, int *);
extern long double frexpl(long double, int *);

//TODO: 提取_X 指数为无符号整型值
extern int ilogbf(float);
extern int ilogb(double);
extern int ilogbl(long double);

//TODO: 以 FLT_RADIX乘以_X到乘幂运算_Y
extern float scalbnf(float, int);
extern double scalbn(double, int);
extern long double scalbnl(long double, int);

//TODO: 将浮点数乘以 FLT_RADIX 的整数幂。
extern float scalblnf(float, long int);
extern double scalbln(double, long int);
extern long double scalblnl(long double, long int);

//TODO: 浮点数绝对值
extern float fabsf(float);
extern double fabs(double);
extern long double fabsl(long double);

//TODO: 计算参数的实立方根
extern float cbrtf(float);
extern double cbrt(double);
extern long double cbrtl(long double);

//TODO: 求值直角三角形的斜边长度，给出的两边为 x 和 y (换言之，根的长度 x2 + y2)。
extern float hypotf(float, float);
extern double hypot(double, double);
extern long double hypotl(long double, long double);

//TODO: 求n的m次方的值,计算以n为底数的m次幂
extern float powf(float, float);
extern double pow(double, double);
extern long double powl(long double, long double);

//TODO: 开平方
extern float sqrtf(float);
extern double sqrt(double);
extern long double sqrtl(long double);

//TODO: 函数返回 x 的高斯误差函数
extern float erff(float);
extern double erf(double);
extern long double erfl(long double);
//https://msdn.microsoft.com/zh-cn/library/
//TODO: 函数返回 x 的补余高斯误差函数。
extern float erfcf(float);
extern double erfc(double);
extern long double erfcl(long double);
```


```
/* ===================
 lgammaf, lgamma, and lgammal are not thread-safe. The thread-safe
 variants lgammaf_r, lgamma_r, and lgammal_r are made available if
 you define the _REENTRANT symbol before including <math.h>           
 ====================== */
#pragma mark ---lgammaf...
//TODO: 计算伽玛参数绝对值的自然对数
extern float lgammaf(float);
extern double lgamma(double);
extern long double lgammal(long double);

//TODO: 计算_X 的伽玛函数
extern float tgammaf(float);
extern double tgamma(double);
extern long double tgammal(long double);

//TODO: 计算值的上限。向上取整 进1
extern float ceilf(float);
extern double ceil(double);
extern long double ceill(long double);

//TODO: 对参数向下取整，返回参数的下限。
extern float floorf(float);
extern double floor(double);
extern long double floorl(long double);

//TODO: 通过使用当前舍入方向，舍入参数为浮点格式的一个整数值，整数。返回舍入的整数值。
extern float nearbyintf(float);
extern double nearbyint(double);
extern long double nearbyintl(long double);

//TODO: 将浮点值舍入到最接近的整数（采用浮点格式），四舍五入函数
extern float rintf(float);
extern double rint(double);
extern long double rintl(long double);

//FIXME: 也是四舍五入
extern long int lrintf(float);
extern long int lrint(double);
extern long int lrintl(long double);

//TODO: 四舍五入法
extern float roundf(float);
extern double round(double);
extern long double roundl(long double);

//TODO: 四舍五入，取最接近的整数
extern long int lroundf(float);
extern long int lround(double);
extern long int lroundl(long double);
```

```
#pragma mark ---truncf...
/* ======================
 long long is not part of C90. Make sure you are passing -std=c99 or
 -std=gnu99 or higher if you need these functions returning long longs     
   ====================== */
//TODO: 截断参数为整数部分，返回参数的整数部分
extern float truncf(float);
extern double trunc(double);
extern long double truncl(long double);

//TODO: 求两数整除后的余数
extern float fmodf(float, float);
extern double fmod(double, double);
extern long double fmodl(long double, long double);

//TODO: 计算余数 _X 除 _Y
extern float remainderf(float, float);
extern double remainder(double, double);
extern long double remainderl(long double, long double);

//TODO: 计算第一个指定参数除以第二个指定参数的余数。也要计算出第一指定参数的有效位数除以第二指定参数的有效位数的商，然后使用在第三参数中指定的位置返回该商。
extern float remquof(float, float, int *);
extern double remquo(double, double, int *);
extern long double remquol(long double, long double, int *);

//TODO: 用_X的大小和_Y的符号来返回值
extern float copysignf(float, float);
extern double copysign(double, double);
extern long double copysignl(long double, long double);

//TODO: 用_X中指示的内容返回清扫NaN，
extern float nanf(const char *);
extern double nan(const char *);
extern long double nanl(const char *);

extern float nextafterf(float, float);
extern double nextafter(double, double);
extern long double nextafterl(long double, long double);

extern double nexttoward(double, long double);
extern float nexttowardf(float, long double);
extern long double nexttowardl(long double, long double);

//TODO: 计算参数之间的正整数差异。（如果 _X 比 _Y 大，_X 和 _Y 之间的差异；否则，+0。）
extern float fdimf(float, float);
extern double fdim(double, double);
extern long double fdiml(long double, long double);

//TODO: 求最大值
extern float fmaxf(float, float);
extern double fmax(double, double);
extern long double fmaxl(long double, long double);

//TODO: 求最小值
extern float fminf(float, float);
extern double fmin(double, double);
extern long double fminl(long double, long double);

//TODO: 计算第一个和第二个指定的参数的产品，然后将第三个指定的参数添加到结果；整个计算是作为单个操作执行的。表达式 (_X x _Y) + _Z 的结果。整个计算都作为单个操作执行，即子表达式计算到无限精度并且仅对最终结果四舍五入。
extern float fmaf(float, float, float);
extern double fma(double, double, double);
extern long double fmal(long double, long double, long double);
```
