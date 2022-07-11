---
title: js学习之 控制台打印console
date: 2016-05-10 16:29:18
tags: JavaScript
categories: [JavaScript]
grammar_cjkRuby: true
---

#### 一、所支持打印类型

%s：代替字符串，%d：代替整数，%f：代替浮点值，%o：代替Object

console清空控制台--`console.clear()`



#### 二、console.debug，info，warn，error

一、显示信息的命令
console.log();  //控制台输入 网页中不会输出
console.info();  //一般信息
console.debug();  //除错信息
console.warn();  //警告提示
console.error();  //错误提示

 “console.log();” 可以用来取代 “alert();” 或 “document.write();” 比如，在网页中写入 “console.log("Hello World");” 然后会在控制台输入，但是网页中并不会输入。方法使用一模一样，只是显示的图标和文字颜色不一样。



#### 三、 console. assert(expression[, object, …])
assert 方法类似于单元测试中的断言，当 expression 表达式为 false 的时候，输出后面的信息,e.g：
注：assert 方法在 firebuglite 不支持，Chrome 和 FireBug 支持
console.clear() -该方法清空 console 中的所有信息
console.dir(object)-以列表的方式打印 object 对象中的所有属性，e.g：
console.dirxml(node)-把 [html](http://liuxiaofan.com/) 元素的[html](http://liuxiaofan.com/) 代码打印出来，e.g:
console.trace()
trace 方法可以查看当前函数的调用堆栈信息，即当前函数是如何调用的。



http://es6.ruanyifeng.com/#docs/destructuring

[JS console方法详解](http://www.leixuesong.cn/1849)

[Firebug控制台详解](http://www.ruanyifeng.com/blog/2011/03/firebug_console_tutorial.html)

[控制台命令Console详解](http://liuxiaofan.com/2013/12/12/1582.html)

