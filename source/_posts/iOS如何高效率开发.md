---
title: iOS如何进行高效率开发
date: 2017-03-16 20:07:40
tags: iOS
grammar_cjkRuby: true
---

##### 1.高效的利用工具

[开发人员的Mac应用速查表](http://www.ctolib.com/cheatsheets-Awesome-Mac.html)

[iOS 高效率编程工具篇](http://huluo666.cn/2016/03/30/iOS%20%E9%AB%98%E6%95%88%E7%8E%87%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7%E7%AF%87/)



##### 2.使用Xcode模板，Code Snippets，代码生成工具

固定的代码不要浪费时间一行一行敲了，使用模板不会出错而且规范。

[iOS自定义Xcode模板](http://www.jianshu.com/p/ba524b5a85d7)



**代码生成工具如**

[JSONExport](https://github.com/Ahmed-Ali/JSONExport)  —直接将json数据转成model文件

如果有必要写些适合自己项目的代码生成工具



##### 3、使用接口调试利器`paw`

Mac最好用的接口调试工具，支持Cookie，历史记录，请求时间等等，支持代码生成，还支持接口文档共享。几乎所有接口相关信息都有，个人觉得比什么Postman好用的多。



##### 4、`MockAPI`模拟数据

一般情况，后台给数据都是比较慢的，给到再调试的时候一是错误多，也很可能忙不过来。

很多人是把模拟数据写死在代码中，反复修改编译不是个好方法。

在线MockAPi工具如：

http://rap.taobao.org/org/index.do

[https://testerhome.com/](https://testerhome.com/)

[http://apizza.cc/?f=lv](http://apizza.cc/?f=lv)

[http://mock-api.com/](http://mock-api.com/)

个人最喜欢用的还是[json-server](https://github.com/typicode/json-server)来模拟API，结合[mock.js](http://mockjs.com/)模拟你想要数据。使用也很简单，基本能满足需要。如果有特殊需求，可以查找更高级的工具。



##### 5、使用谷歌搜索

有问题谷歌，搜不到想要结果翻译成英文搜，谷歌访问不了，可以用镜像或SS，SS太多不知哪个速度快，可以用个人写的[Mac批量Ping工具](http://www.jianshu.com/p/d98d5053db86)。





