---
title: Java学习 类型转换与运算 02
date: 2016-09-28 18:24:12
tags: Java
categories: [Java]
grammar_cjkRuby: true
---

```
class Untitled {
	public static void main(String[] args) {
		System.out.println("hello world");
		
		int x;
		double y;
		x = (int)34.56 + (int)11.2;  // 丢失精度 34 + 11 =45
		y = (double)x + (double)10 + 1;  // 提高精度 35 +10 +1
		System.out.println("x=" + x); //输出结果 x=45
		System.out.println("y=" + y);//输出结果 y=56.0
		
		
		//		自增减法
		int a=10;
		int b=10;
	       
		System.out.println("后自加 a=" + (a++));//10
		System.out.println("a的值 a=" + a);
		System.out.println("c的值 c=" + c);

		System.out.println("前自加 b=" + (++b));
//		++VAR被称为前自加，其后面的变量执行自加操作，其运算为，先执行自加操作，再引用VAR值。
//		VAR++被称为后自加，其前面的变量执行自加操作，其运算为，先引用VAR值，再进行自加操作。

	}
}
```





