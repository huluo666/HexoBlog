---
title: 安卓开发-java基础基本数据类型1
tags: Java
categories: Java
date: 2017-08-02 18:08:43
---

http://www.weixueyuan.net/view/5950.html

```java
import  java.util.Date;
class Untitled {
	public static void main(String[] args) {
		System.out.print("打印Java各基本数据类型所占节数\n");

		System.out.println("Integer: "+Integer.SIZE/8); //4
		System.out.println("Short: "+Short.SIZE/8); 	//2
		System.out.println("Long: "+Long.SIZE/8);		//8
		System.out.println("Byte: "+Byte.SIZE/8);		//1
		System.out.println("Character: "+Character.SIZE/8); //2
		System.out.println("Float: "+Float.SIZE/8);		//4
		System.out.println("Double: "+Double.SIZE/8); 	//8


	    System.out.println("基本类型：int 二进制位数：" + Integer.SIZE);
		System.out.println("包装类：java.lang.Integer");
		System.out.println("最小值：Integer.MIN_VALUE=" + Integer.MIN_VALUE);
		System.out.println("最大值：Integer.MAX_VALUE=" + Integer.MAX_VALUE);
	}
}
```

http://blog.csdn.net/huwenhu2007/article/details/9057569



OC

```objectivec
#import <limits.h>
NSLog(@"NSInteger: %ld", sizeof(NSInteger));
    NSLog(@"CGFloat: %ld", sizeof(CGFloat));
    NSLog(@"double: %ld", sizeof(double));
    NSLog(@"char: %ld", sizeof(char));
    NSLog(@"int: %ld", sizeof(int));
    NSLog(@"float: %ld", sizeof(float));
    NSLog(@"short int: %ld", sizeof(short int));


    NSLog(@"CHAR_MIN:   %c",   CHAR_MIN);
    NSLog(@"CHAR_MAX:   %c",   CHAR_MAX);
    NSLog(@"SHRT_MIN:   %i",  SHRT_MIN);    // signed short int
    NSLog(@"SHRT_MAX:   %i",  SHRT_MAX);
    NSLog(@"INT_MIN:    %i",   INT_MIN);
    NSLog(@"INT_MAX:    %i",   INT_MAX);
    NSLog(@"LONG_MIN:   %li",  LONG_MIN);    // signed long int
    NSLog(@"LONG_MAX:   %li",  LONG_MAX);
    NSLog(@"ULONG_MIN not defined, it's always zero: %d", 0);
    NSLog(@"ULONG_MAX:  %lu",  ULONG_MAX);   // unsigned long int
    NSLog(@"LLONG_MIN:  %lli", LLONG_MIN);   // signed long long int
    NSLog(@"LLONG_MAX:  %lli", LLONG_MAX);
    NSLog(@"ULLONG_MIN not defined, it's always zero: %d", 0);
    NSLog(@"ULLONG_MAX: %llu", ULLONG_MAX);  // unsigned long long int
```

http://blog.csdn.net/minggeqingchun/article/details/52230467