---
title: iOS开发自定义NSLog
date: 2017-03-22 17:06:56
tags: iOS
grammar_cjkRuby: true
---

##### 屏蔽Xcode烦人Log

Xcode8之后，会打印一些烦人的Log信息

解决方法：1- Xcode菜单按钮: `Product > Scheme > Edit Scheme`>`Run`>`Arguments`或者直接按 `command + shift + <` 快捷键

​		   2- **Environment Variables** 设置为 `OS_ACTIVITY_MODE` = `disable`

stackoverflow:[hide-strange-unwanted-xcode-8-logs](http://stackoverflow.com/questions/37800790/hide-strange-unwanted-xcode-8-logs)

##### C语言宏定义

| 符号             | 类型     | 描述                                      |
| -------------- | ------ | --------------------------------------- |
| `__FILE__`     | String | 当前文件的绝对路径                               |
| `__LINE__`     | Int    | 展开该宏时在文件中的行数                            |
| `__COLUMN__`   | Int    | 展开该宏时在文件中的列                             |
| `__FUNCTION__` | String | 包含这个符号的函数名称                             |
| `__COUNTER__`  | Int    | 无重复的计数器，从程序启动开始每次调用都会++，常用语宏中定义无重复的参数名称 |

```
NSLog(@"%s %d %s %s", __FILE__, __LINE__, __PRETTY_FUNCTION__, __FUNCTION__);
```



`printf()`和`fprintf()`这些输出函数的参数是可变的

C99中规定宏可以像函数一样带有可变参数：

```
#define LOG(format, ...) fprintf(stdout, format, __VA_ARGS__)
```

其中，`...`表示参数可变，`__VA_ARGS__`在预处理中为实际的参数集所替换



GCC中同时支持如下的形式

```
#define LOG(format, args...) fprintf(stdout, format, args)
```

其用法和上面的基本一致，只是参数符号有变化

"`##`"的作用是对token进行连接，在上例中，`format`、`__VA_ARGS__`、`args`即是token，

如果token为空，那么不进行连接，所以允许省略可变参数(__VA_ARGS__和args)，对上述变参宏做如下修改

```
#define LOG(format, ...)     fprintf(stdout, format, ##__VA_ARGS__)
#define LOG(format, args...) fprintf(stdout, format, ##args)
```

```objectivec
//关键字
...:可变参数
 __VA_ARGS__ :宏定义中的...中的所有剩余参数
 ##:连接符号
 #:原样输出
/:换行符
```



```objectivec
#define K_AAAAA(aaa,...) aaa
#define K_BBBBB(...) K_AAAAA(__VA_ARGS__)

 //调用示例：
 NSLog(@"outPut :%@",K_BBBBB(@"this is string"));
 NSLog(@"outPut2 :%d",K_BBBBB(100));
 //输出： outPut :this is string
        outPut2 :this is 100
```



##### Xcode8后的自定义Log

```objectivec
#define NNSLog00(format,...) NSLog(format,__VA_ARGS__);
#define NNSLog0(fmt, ...) NSLog((fmt), ##__VA_ARGS__);
#define NNSLog(...) fprintf(stderr,"%s\n",[[NSString stringWithFormat:__VA_ARGS__] UTF8String]);
#define NNSLog1(FORMAT,...) fprintf(stderr,"%s\n",[[NSString stringWithFormat:FORMAT,##__VA_ARGS__] UTF8String]);
#define NNSLog2(...) fprintf("%s %s Line %d: %s\n",[[NSString stringWithFormat:@"%s", __FILE__].lastPathComponent UTF8String], __PRETTY_FUNCTION__, __LINE__, [[NSString stringWithFormat:__VA_ARGS__] UTF8String]);
```



##### 格式化宏

可以/换行符格式化宏，如：

```
#define NSLog(format, ...) \
    do { \
        NSLog(@"<%@ : %d : %s>-: %@", \
        [[NSString stringWithUTF8String:__FILE__] lastPathComponent], \
        __LINE__, \
        __FUNCTION__, \
        [NSString stringWithFormat:format, ##__VA_ARGS__]); \
    } while(0)


```



##### 控制Log输出

```objectivec
#ifdef DEBUG
#define NSLog(fmt, ...) NSLog((fmt), ##__VA_ARGS__);
#else
#define NSLog(...) {}
//#define NSLog(format, ...) do{ } while(0)

#endif
```



如果在项目中log信息太多，可以屏蔽共用log，使用自定义log，便于局部测试

```
#define NSLog(...) {}//屏蔽共用log
#define NNSLog(...) fprintf(stderr,"%s\n",[[NSString stringWithFormat:__VA_ARGS__] UTF8String]);//使用自定义log
```

参考

https://onevcat.com/2014/01/black-magic-in-macro/