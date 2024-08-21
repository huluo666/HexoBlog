---
title: java语法学习 1
date: 2016-09-23 12:12:37
tags: java
categories: [Java]
grammar_cjkRuby: true
---

学习来源：http://www.weixueyuan.net/java/rumen_1/

创建Test.java 文件

```java
import  java.lang.*;//编译器默认已导入jar包，System类可使用
public  class Test{
		public static void main(String[] args){
			System.out.print("gghgh");

			   // 字符型
				char webName1 = '微';
				char webName2 = '学';
				char webName3 = '苑';
				System.out.println("网站的名字是：" + webName1 + webName2 + webName3);
			       
				// 整型
				short x=22;  // 十进制
				int y=022;  // 八进制
				long z=0x22L;  // 十六进制
				System.out.println("转化成十进制：x = " + x + ", y = " + y + ", z = " + z);
			       
				// 浮点型
				float m = 22.45f;
				double n = 10;
				System.out.println("计算乘积：" + m + " * " + n + "=" + m*n);
				
				boolean aa =100 > 10;
				boolean bb =100 < 10;
				System.out.println("100>10 = " + aa);
				System.out.println("100<10 = " + bb);
				   
				if(aa){
					System.out.println("100<10是对的");
				}else{
					System.out.println("100<10是错的");
				}		
				
				
				//定义一个名为student类
				class Student{  // 通过class关键字类定义类
					// 类包含的变量
					String name;
					int age;
					float score;
					// 类包含的函数
					void say(){
						System.out.println( name + "的年龄是 " + age + "，成绩是 " + score);
					}
					
					void sayhelloworld(){
						System.out.print(name + "年龄 " +age +"成绩 " +score);
						
					}
				}
				// 通过类来定义变量，即创建对象
				Student stu1 = new Student();  // 必须使用new关键字
				// 操作类的成员
				stu1.name = "小明";
				stu1.age = 15;
				stu1.score = 92.5f;
				stu1.say();
				
				Student stu2 = new Student();  // 必须使用new关键字
				// 操作类的成员
				stu2.name = "小吉";
				stu2.age = 16;
				stu2.score = 94.7f;
				stu2.sayhelloworld();
		}
}
```


**Java中print、printf、println的区别**

printf主要是继承了C语言的printf的一些特性，可以进行格式化输出
print就是一般的标准输出，但是不换行
println和print基本没什么差别，就是最后会换行
System.out.printf("the number is: d",t);

**参照JAVA API的定义如下：**'d' 整数 结果被格式化为十进制整数
'o' 整数 结果被格式化为八进制整数
'x', 'X' 整数 结果被格式化为十六进制整数
'e', 'E' 浮点 结果被格式化为用计算机科学记数法表示的十进制数
'f' 浮点 结果被格式化为十进制数
'g', 'G' 浮点 根据精度和舍入运算后的值，使用计算机科学记数形式或十进制格式对结果进行格式化。
'a', 'A' 浮点 结果被格式化为带有效位数和指数的十六进制浮点数
println("test")相当于print("testn")就是一般的输出字符串



```java
   int i = 4;
   double j = 5;
   System.out.print("用print输出i:"+ i);
   System.out.println( "用println输出i:"+ i);
   System.out.printf("i的值为%d,j的值为%f", i,j);
```




断点调试命令

直接在控制台输入help，即可看到相关调试命令

