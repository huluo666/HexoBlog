---
title: Android开发-Java基础1
tags: Java
categories: Java
date: 2017-08-02 18:08:43
---

Java学习

```java
//http://www.weixueyuan.net/view/5947.html
//注意Java是大小写敏感的
//class --java 文件名
public class HelloWorldDemo {
	public static void main(String[] args) {
		System.out.print("Hello world!");
		System.out.print("\n打印中文！！\n");
		
		
		    // 定义类Student
			class  Student{
				String name;
				int age;
				double score;
				void say(){
					//println和print基本没什么差别，就是最后会换行
					System.out.println( name + "的年龄是 " + age + "，成绩是 " + score );
				}
			}
				
				
			//通过类定义变量，即创建对象
			Student  stu1 = new  Student();//必须使用new关键字
			//成员赋值
			stu1.name = "小明";
			stu1.age=16;
			stu1.score=92.5;
			stu1.say();//调用函数
			
	}
}
```

