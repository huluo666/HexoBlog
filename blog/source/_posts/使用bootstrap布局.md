---
title: 使用bootstrap布局
tags: JavaScript
categories: JavaScript
date: 2017-04-05 14:17:37
grammar_cjkRuby: true
---

使用`bootstrap`布局

[Bootstrap vs. Foundation vs. Skeleton](http://blog.csdn.net/iefreer/article/details/9736967)

`Bootstrap` 和`Foundation`, `Skeleton`一样是响应式WEB界面框架。

bootstrap官网

http://www.bootcss.com/

http://v3.bootcss.com/



优站精选

http://expo.bootcss.com/

Bootstrap 教程

http://www.runoob.com/bootstrap/bootstrap-tutorial.html



### 一、创建App工程

用express创建工程

```shell
$ express -t  newsproject     #默认使用pug模板
#express -t ejs newsproject   #指定使用ejs模板
```

进入目录运行npm安装

```shell
$ cd newsproject  
$ npm install 
```

运行项目

```shell
$ DEBUG=newsproject:* npm start
#node ./bin/www
```





### 二、集成bootstrap布局

在node目录(app.js目录)下执行

```shell
$ npm install bootstrap --save
$ npm install jquery --save	#由于bootstrap需要jquery,发现默认已安装，可忽略
```

- 打开nodeproject下的app.js，找到`app.use(express.static(path.join(__dirname, 'public')));`在下一行添加如下代码:

  ```javascript
  app.use(express.static(path.join(__dirname, 'public')));
  app.use('/lib',express.static(path.join(__dirname, 'node_modules')));
  ```

这样就把`node_modules`下的文件映射为我们的静态资源文件了。

- 1、在`nodeproject/views`目录下创建一个`includes`目录，在includes创建一个`head.jade`文件，写入以下内容：

  ```jade
  link(href="/lib/bootstrap/dist/css/bootstrap.min.css", rel="stylesheet")
  script(src="/libs/jquery/dist/jquery.min.js") 
  script(src="/libs/bootstrap/dist/js/bootstrap.min.js")

  <!--使用以下代码也可以  -->
  <!-- script(src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js") -->
  <!-- script(src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js") -->
  ```

- 2、在`layout.jade`引入`head.jade`

  ```jade
  doctype html
  html
    head
      meta(charset="utf-8")
      title= title
      include ./includes/head
    body
      block content
  ```



在index.js内容如下

```javascript
var express = require('express');
var router = express.Router();

/* GET home page. */
//index page
//路由规则和URL地址相匹配
router.get('/', function(req, res, next) {
    res.render('index', {   //渲染模板
        title: '豆瓣电影 首页',
        movies: [{  //伪造模板数据
            title: '血战钢锯岭 Hacksaw Ridge',
            _id: 1,
            poster: 'https://img1.doubanio.com/view/movie_poster_cover/lpst/public/p2397337958.jpg'
        },{
            title: '血战钢锯岭 Hacksaw Ridge',
            _id: 2,
            poster: 'https://img1.doubanio.com/view/movie_poster_cover/lpst/public/p2397337958.jpg'
        },{
            title: '血战钢锯岭 Hacksaw Ridge',
            _id: 3,
            poster: 'https://img1.doubanio.com/view/movie_poster_cover/lpst/public/p2397337958.jpg'
        },{
            title: '血战钢锯岭 Hacksaw Ridge',
            _id: 4,
            poster: 'https://img1.doubanio.com/view/movie_poster_cover/lpst/public/p2397337958.jpg'
        },{
            title: '血战钢锯岭 Hacksaw Ridge',
            _id: 5,
            poster: 'https://img1.doubanio.com/view/movie_poster_cover/lpst/public/p2397337958.jpg'
        },{
            title: '血战钢锯岭 Hacksaw Ridge',
            _id: 6,
            poster: 'https://img1.doubanio.com/view/movie_poster_cover/lpst/public/p2397337958.jpg'
        }]
    });
});

module.exports = router;
```



index.jade文件内容如下

```jade
extends layout

block content
	.container
		.row
			each item in movies
				.col-md-2
					.thumbnail
						a(href="/movie/#{item._id}")
							img(src="#{item.poster}", alt="#{item.title}")
						.caption
							h3 #{item.title}
							p: a.btn.btn-primary(href="/movie/#{item._id}", role="button") 观看
```



再次运行

```shell
$ sudo DEBUG=myvideo:* npm start
```

访问http://localhost:3000/

![效果图](http://7xr7vj.com1.z0.glb.clouddn.com/UC20170405_150634.png)

### 参考

[从零开始搭建Nodejs,Express,Ejs,bootstrap,VueJs,Mongodb运行环境教程](http://wiliam.me/2016/12/31/20161231162411.html)