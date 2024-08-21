---
title: 使用express创建自己第一个nodejs程序
date: 2017-02-10 18:24:12
tags: Node.js
categories: Node.js
grammar_cjkRuby: true
---

**1.安装Express**

```
npm install express -g
npm install express-generator -g
```

说明：`express`是web框架

​	`Express-generator` （Express 应用生成器）,通过该工具，可以快速创建一个express应用的骨架。

​    Express4.0+版后将`Express-generator`命令工具分离了，所以你必须安装`express-generator`才能生成express应用。



##### 2、创建项目基于express的项目

使用命令进入创建文件夹的目录（~/Documents/iOSLive）

```
 cd ~/Desktop 	//进入桌面
 express 项目名称 --view=ejs //创建app使用ejs模板-默认jade模板
```



##### 3、node模块安装

 进入项目所在的目录下，执行命令npm install

```
cd package.json文件所在目录
npm install   //安装依赖
//若是想要加载其他模块，可在package.json中添加相应的信息
```



##### 4、启动服务

```
npm start //或node ./bin/www
```

浏览器输入http://127.0.0.1:3000/可看到`Welcome to Express`则表示成功。





##### 更多高级用法

http://stackoverflow.com/questions/3855127/find-and-kill-process-locking-port-3000-on-mac

https://www.teakki.com/p/57dfa7fe3c20b02e90a0cfae

[Express.js 创建Node.js Web应用](https://itbilu.com/nodejs/npm/EJUJrGVsg.html)

https://www.zybuluo.com/XiangZhou/note/207341

 [Node.js历险记之express框架入门篇](http://jishu.y5y.com.cn/gamer_gyt/article/details/60151783)



##### Jade模板引擎教程

http://blog.jayself.com/2014/07/28/Jade/

http://cabins.github.io/2016/04/15/Jade%E6%A8%A1%E6%9D%BF%E5%BC%95%E6%93%8E%E6%95%99%E7%A8%8B/

天气demo

http://a5566baga.cn/2017/02/15/Node-js%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E6%98%BE%E7%A4%BA%E5%A4%A9%E6%B0%94%E7%9A%84%E6%9C%8D%E5%8A%A1/

http://smallyard.cn/2015/11/04/tianqi/



[从零开始搭建Nodejs,Express,Ejs,bootstrap,VueJs,Mongodb运行环境教程(一)](http://wiliam.me/2016/12/22/20161222132357.html)



Demo

https://github.com/SilentSword69/website-demo/tree/master

https://github.com/popingpaul/movie

##### 起步

简要介绍 Bootstrap，以及如何下载、使用，还有基本模版和案例，等等。

http://v3.bootcss.com/getting-started/