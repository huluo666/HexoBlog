---
title: 利用gulp和browser-sync实现Pug(jade)的实时预览
tags: node.js
categories: JavaScript
date: 2017-04-01 14:43:51
grammar_cjkRuby: true
---

jade由于和注册商标重名，更名为pug。

##### 一、Pug安装编译

`$ npm install -g pug #全局安装`

这里如果遇见了

`bash: pug: command not found`

还需要安装`pug-cli`，具体原因，Github给出的解释是这样的：

```
You need to install pug-cli. The CLI was separated from the core library as part of this release.
```

```shell
$ npm install -g pug-cli
```

cli是命令行端的程序。我们通过命令行来编译pug文件。



##### pug简单使用：

`pug -h`— 查看帮助

`pug demo.pug`---在同级目录渲染并生成html文件

```shell
常用参数说明:
-o 制定html输出目录
-P 格式化输出html文件（样式美观）
-w 监听模式，一旦pug文件有修改，自动会再次渲染输出html文件
-b 制定所有pug文件的根目录用来指定文件的绝对路径
-p 使用绝对路径方式
```



#### 二、gulp和browser-sync安装

```shell
$ sudo npm install gulp-pug
$ sudo npm install browser-sync gulp --save-dev  
#—save-dev这个属性会将条目保存到你package.json的devDependencies里面
```

在当前目录 创建package.json和gulpfile.js

```shell
$ node init   #创建package.json文件
```

1.package.json文件配置：

```
{
	"name": "gulp",
	"version": "3.9.1",
	"description": "配置package文件，实现文件更改后浏览器即时刷新",
	"main": "gulpfile.js",
	"scripts": {
		"test": "echo \"Error: no test specified\" && exit 1"
	},
	"keywords": [
		"gulp"
	],
	"author": "fidding.hjh",
	"devDependencies": {
		"browser-sync": "^2.10.0",
		"gulp": "^3.9.1"
	}
}
```

注意里面的版本号要与安装的版本相对应



2.gulpfile.js配置：

```javascript
var gulp        = require('gulp');
var browserSync = require('browser-sync').create();
var pug = require('gulp-pug');

/**
 * 编译pug
 */
gulp.task('task_pug', function() {
	console.log(pug);
	return gulp.src('./src/*.pug')
		.pipe(pug({pretty:true})) //格式化
		.pipe(gulp.dest('./src')) //输出到文件夹
});



/*静态页面html实时预览*/
gulp.task('browser-sync', function() {
	browserSync.init({
		server: "./src"	////指定服务器启动根目录  
	});
	
	gulp.watch('src/*.pug', ['task_pug']);	//监听pug文件变化-执行task_pug方法使用gulp-pug编译pug
	gulp.watch(['src/*.html',]).on("change",browserSync.reload);//监听pug文件变化 刷新浏览器
});

gulp.task('default',['task_pug','browser-sync']); //定义默认任务 命令 gulp default
```

执行 `$ gulp default` 会打开http://localhost:3000 页面，如果出现Can't get  url 页面，需要在src目录下添加index.html页面。

- `gulp` 等价于`gulp default`,运行default中定义的所有任务

  ​

Mac系统可以使用[http-server](https://www.npmjs.com/package/http-server)或Python开启 `Web Server`,当前工作目录下执行

```
$ python -m SimpleHTTPServer 3000    #指定与gulp相同端口3000
```

把生成的网页目录以文件名`index.html`保存到`src`目录下即可

这样每次修改pug文件会自动编译成html文件，并利用`browser-sync`实时刷新预览。



#### 三、出错解决：

[Can't get Gulp to run: cannot find module 'gulp-util'](http://stackoverflow.com/questions/21406738/cant-get-gulp-to-run-cannot-find-module-gulp-util)



#### 参考：

- [gulp 基本使用](http://www.jianshu.com/p/61b31dbf030d)


- [gulp配置完全指南](http://blog.leanote.com/post/github-dtcz/gulp%E9%85%8D%E7%BD%AE%E5%AE%8C%E5%85%A8%E7%AF%87)


- [Gulp使用指南](http://www.techug.com/post/gulp.html)

- [gulp plugins 插件介绍](http://colobu.com/2014/11/17/gulp-plugins-introduction/)
- [gulp.watch的两种使用方法](http://www.jianshu.com/p/6f85a44d01f4)



