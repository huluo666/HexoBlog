---
title: iOS 自定义NSLog
date: 2016-08-10 17:34:43
tags: iOS
grammar_cjkRuby: true
---

宏定义、NSLog我们经常用到，但系统默认的NSLog往往不能满足我们开发的需求。譬如以下开发常见自定义NSLog就可以帮助我们获取更多的相关信息。

#### 1、自定义NSLog代码示例

```
#ifdef DEBUG
#define NSLog(fmt, ...) NSLog((@"%s [Line %d] " fmt), __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__)
#else
#define NSLog(...)
#endif
```



```
#define NSLog(format, ...) \
    do { \
        NSLog(@"<%@ : %d : %s>-: %@", \
        [[NSString stringWithUTF8String:__FILE__] lastPathComponent], \
        __LINE__, \
        __FUNCTION__, \
        [NSString stringWithFormat:format, ##__VA_ARGS__]); \
    } while(0)
    
    
#ifdef DEBUG
#define NSLog(format, ...) \
    //Log定义...
#else
    #define NSLog(format, ...) do{ } while(0)
#endif
```



#### 2、如何控制调试/发布模式开关

```
 #ifdef DEBUG
 #   define NSLog(...) NSLog(__VA_ARGS__)
 #else 
 #   define NSLog(...)
 #endif
```



建议使用自定义Release/DEBUG 模式开关，如下：

```
#warning 发布时，将APP_CUSTOM_DEBUG设置为0！!(0-发布  1-生产)

#define APP_CUSTOM_DEBUG 0


#if APP_CUSTOM_DEBUG
#define NEWS_NOTIFY_URL   @"http://192.168.240.113.xxx"//测试环境URL
#else
#define NEWS_NOTIFY_URL   @"http://xxxxbbbb.com.cn/xxx"//正式环境URL 线上地址
#endif


#if APP_CUSTOM_DEBUG
#   define NSLog(...) NSLog(__VA_ARGS__)
#else
#   define NSLog(...)
#endif
```



#### 3.相关参数释义

**1）ifdef #ifndef #if...含义**

- `#ifdef` - If this macro is defined（该宏已定义）
- `#ifndef` - If this macro is not defined（该宏未定义）
- `#if` - Test if a compile time condition is true（如果一个编译时条件是真的）
- `#else` - The alternative for #if （选择-如果）
- `#elif` - #else an #if in one statement（#其他#如果一个语句）
- `#endif` - End preprocessor conditional（结束预处理条件）




**2）宏定义参数释义**

1、`__FILE__`   		宏在预编译时会替换成当前的`源文件名`

2、`__LINE__`		宏在预编译时会替换成当前的`行号`
3、`__FUNCTION__`	宏在预编译时会替换成当前的`函数名称`

4、`__VA_ARGS__`		是一个`可变参数`的宏，这个可变参数的宏是新的C99规范中新增的，目前似乎只有gcc支（VC6.0的编译器不支持）。宏前面加上`##`的作用在于，当可变参数的个数为0时，这里的`##`起到把前面多余的"`,`"去掉的作用,否则会编译出错。



`相关扩展阅读`：

Swift Log https://gist.github.com/xmzio/fccd29fc945de7924b71

[Swift print, println, NSLog](http://www.knowstack.com/swift-print-println-nslog/)

[Enable and Disable NSLog in DEBUG mode](http://stackoverflow.com/questions/6552197/enable-and-disable-nslog-in-debug-mode)

