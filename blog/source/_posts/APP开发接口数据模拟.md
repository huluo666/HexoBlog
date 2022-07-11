---
title: APP开发接口数据模拟
date: 2016-12-18 18:24:12
tags: 工具
categories: 工具
grammar_cjkRuby: true
---

测试API的模拟主要分以下2块

- **1.模拟服务器** 
- **2.模拟测试数据**

### 一、模拟服务器解决方案

#### 1）.使用`Apache`开启 `Web Server` 

Mac自己集成了`Python`和`Apache`

启动：`sudo apachectl start`

```shell
停止：sudo apachectl stop
重启：sudo apachectl restart
查看 Apache 版本： httpd -v
```

浏览器打开 http://127.0.0.1 可以看到 **It works!** 的页面.

1. 使用本地回环测试地址http://127.0.0.1
2. 使用http://locahost

mac下Apache的默认文件夹为`/Library/WebServer/Documents`

在该目录添加一个名为test.json文件

浏览器输入http://127.0.0.1/test 即可看到test.json文件内容



**修改Apache目录**

以上说的有点杂，折腾很久还是出现`Forbidden You don't have permission to access / on this server.`

　上面说到了mac下Apache的默认文件夹为`/Library/WebServer/Documents`，该目录默认是隐藏的，操作不是很方便，我们可以将其修改成自定义的目录。

- **1.1**、打开终端，输入命令：`sudo vim /etc/apache2/httpd.conf`

也可以找到`httpd.conf`文件进行编辑

```shell
$ cd /etc/apache2   #进入文件夹
$ open .		    #打开文件夹
```

- **1.2**、找到如下两处

　　`DocumentRoot "/Library/WebServer/Documents"`
　　`<Directory "/Library/WebServer/Documents">`

- **1.3**、将两处中引号中的目录替换为自定义的目录，如"`/Library/apacheWeb`"

完成以上三步后，重启Apache，浏览器输入http://127.0.0.1

`Forbidden You don't have permission to access / on this server.`

这个折腾了好久，修改权限`sudo chmod -R 777 apacheWeb`也不行，灵机一动把`/Library/WebServer/Documents`目录中的`index.html.en`文件拷贝到自定义的目录，`sudo  apachectl restart` 后就可以看到 **It works!** 的页面！

访问某一文件如：http://127.0.0.1/db



**其它修改方式参考**

[Mac 下修改 PHP 本地服务器路径](http://www.saitjr.com/php/php-mac-yosemite-locahost-path.html)

[mac 升级后配置 apache 到个人目录](http://www.jianshu.com/p/baa1102490eb)

#### 2）.使用Python开启 `Web Server` 

相比Apache更简单不需要改本地服务器路径，随便进入一个目录即可开启

```shell
$ python -m SimpleHTTPServer 		 #默认8000端口
$ python -m SimpleHTTPServer 8080    #指定端口为8080
```

`python`会以当前目录作为根目录起一个本地server, 访问`localhost:8000`就可以看到效果了。



#### 3）使用Node.js 模拟服务器

##### **3.1** [json-server](https://github.com/typicode/json-server)

3.1.1、全局`json-server`安装

```shell
$ sudo npm install json-server -g
```

安装完成后可以用 `json-server -h` 命令检查是否安装成功，成功后会出现帮助命令选项。

3.1.2.启动server

```shell
$ cd  ~/Desktop/mock 				#进入db.js文件目录
$ json-server --watch db.json	    #启动监听服务,当然也可以监听js,json，text，md等文本文件
```

如果成功会出现:

```
 \{^_^}/ hi!
 {xxx: 'xxx'}
  Home
  http://localhost:3000
```

`json-server`启动默认端口为3000；

这个时候访问 http://localhost:3000/db可以查看所定义的全部数据。

更多：[前后端分离下的接口数据模拟](http://fnpyud.com/2016/06/%E5%89%8D%E5%90%8E%E7%AB%AF%E5%88%86%E7%A6%BB%E4%B8%8B%E7%9A%84%E6%8E%A5%E5%8F%A3%E6%95%B0%E6%8D%AE%E6%A8%A1%E6%8B%9F/)





3.2 **使用http-server搭建静态服务器**

1.安装http-server

```
npm install http-server -g
```

2.启动

```shell
http-server -a 127.0.0.1				#默认端口8080
http-server -a 127.0.0.1 -p 9999		#指定端口9999
```

更多：https://www.npmjs.com/package/http-server



##### 3.2 [ohana](https://github.com/Allenice/ohana)

`ohana` 是一个返回模拟 json 数据的 node http 服务器，默认集成了 mockjs 生成动态的 json 数据，支持 POST, GET, PUT, DELETE 四种请求。

**特点：**

- 使用 mockjs 生成 json 数据

- 支持路由规则

- 可跨域访问

  如何使用：

  作者主页http://blog.allenice233.com/2014/12/01/ohana-node-server/



常用web服务器框架

- [express](http://expressjs.com/)
- [hapi](http://hapijs.com/)
- [koa](http://koajs.com/)
- [restify](http://restify.com/)



开源项目：

- [local-ajax-api](https://github.com/kliuj/local-ajax-api)

- [imitator](https://github.com/hanan198501/imitator)

- [esky-mock](https://www.npmjs.com/package/esky-mock)

- [aspserver](https://www.npmjs.com/package/aspserver)  帮助快速搭建一个服务器，并自动拥有目录浏览等功能。

- [fie-plugin-mock](https://github.com/fieteam/fie-plugin-mock)

  以上是个人使用过的一些库，还有更多好用的库可以在https://www.npmjs.com中搜索

  ​

### 二、模拟数据生成

##### 1.常用node.js数据模拟库

[faker.js](https://github.com/marak/Faker.js/)

[mock.js](http://mockjs.com/)



##### 2.生成模拟 JSON 在线工具:

- [JSON Generator](http://www.json-generator.com/)

- [Mockaroo](https://www.mockaroo.com/)

  ​

##### 3.JSON API 在线模拟工具:

- [Mocky](http://www.mocky.io/)

- [jsonohyeah](http://www.jsonohyeah.com/)

- [JSONPlaceholder](http://jsonplaceholder.typicode.com/)

- [mockable.io](https://www.mockable.io/)

- [FillText.com](http://www.filltext.com/)

  ​

  商业化方案

  - [http://apizza.cc/?f=lv](http://apizza.cc/?f=lv)
  - [https://apiary.io/](https://apiary.io/)
  - [http://www.easyapi.com/](http://www.easyapi.com/)
  - http://mock-api.com/
  - [ http://rap.taobao.org/org/index.do](https://www.xgllseo.com/%20http://rap.taobao.org/org/index.do)

##### 代理服务器

- 使用 [charles](http://www.charlesproxy.com/)作为代理服务器
- 使用代理服务器的 map（映射）& rewrite（重写）功能




**示例代码：**

`mock.js` 生成模拟数据，其它如json-server，koa，express等直接引用生成的数据就行

```javascript
// 使用 Mock 保存为mock_users.js
var Mock = require('mockjs')
let Random = Mock.Random;

function generateCustomers () {
	var total=10;
	var customers = []
	for (var id = 0; id < total; id++) {
		var firstName =Random.cname();
		var email =Random.email("pconline.com.cn");
		var gender = Random.pick(['男', '女']);		
		customers.push({
			"id": id,
			"first_name": firstName,
			"Gender": gender,
			'email': email,
		})
	}
	var data = Mock.mock({ "customers": customers})
	return {"customers": customers,
			"stateCode":"200",
			"total":"85"
			}
}
var jsonData = JSON.stringify(generateCustomers());//转换为json字符格式,在服务器端直接解析req.body  
exports.mockJson = function () {
	return jsonData;
};

exports.mockJson2 = function () {
	return generateCustomers();
};


exports.mockJsonTest = function () {
	return  {"customers":"hello world"};
};
```



使用`json-server`模拟http server

```javascript
//保存为 jsonserverMockAPI.js
var customUser = require('./mock_users2.js');//引入模拟数据js文件
// 如果你要用json-server的话，就需要export这个生成fake data的function
module.exports = function() {
//	var jsonData = JSON.stringify(generateCustomers2());//转换为json字符格式,在服务器端直接解析req.body  
	return customUser.mockJson3();
}
```

运行服务 `json-server --watch jsonserverMockAPI.js`





使用`express`模拟http server

```javascript
//保存为 expressMockAPI.js
// 引入 `express` 模块
var express = require('express');
// 调用 express 实例
var app = express();
var customUser = require('./mock_users.js');//引入模拟数据js文件
//console.log( '生成模拟数据\n' + customUser.mockJson());

var jsonData1 = customUser.mockJson();//转换为json字符格式,在服务器端直接解析req.body  
// app 本身有很多方法，其中包括最常用的 get、post、put/patch、delete，在这里我们调用其中的 get 方法，为我们的 `/` 路径指定一个 handler 函数。
// req和res是reques和response的缩写
var jsonData = customUser.mockJson2();//转换为json字符格式,在服务器端直接解析req.body  


app.get('/', function (req, res) {
	res.contentType('json');//返回的数据类型  
		res.send(jsonData);//给客户端返回一个json格式的数据  
		res.end();  
});
 
// 监听本地的 3000 端口
app.listen(3000, function () {
	console.log('监听3000端口');
});
```

express服务器启动 `$ node expressMockAPI.js`



使用`koa`模拟http server

```javascript
//保存为如：KoaMockAPI001.js
//使用教程 https://cnodejs.org/topic/5709959abc564eaf3c6a48c8 
var customUser = require('./mock_users.js');//引入模拟数据js文件
console.log( '生成模拟数据\n' + customUser.mockJson());

var Koa = require('koa');
var app = new Koa();
// 此处开始堆叠各种中间件
//...

// response
app.use(ctx => {
	ctx.body = customUser.mockJson();
});

app.listen(3000);
```



如何自动化

可以用如`nodemon` 去启动你的 express

 [https://github.com/remy/nodemon](https://github.com/remy/nodemon)

```
nodemon app.js
```





### Related Posts:

- [用JSON-server模拟REST API(一) 安装运行系列](http://www.cnblogs.com/lewo/p/mock-json-server-install.html)


- [ohana - 一个返回模拟 json 数据的 node http server](http://blog.allenice233.com/2014/12/01/ohana-node-server/)

- [forever让NodeJs应用后台执行](https://cnodejs.org/topic/5021c2cff767cc9a51e684e3)

- [网站后台要做客户端API接口，接口文档如何写?](https://www.zhihu.com/question/23010264)  知乎

  ​

