---
title: 使用express创建自己第一个nodejs程序二
date: 2017-02-10 18:24:12
tags: Node.js
categories: Node.js
grammar_cjkRuby: true
---

一、添加和安装依赖

`package.json`这个基本的JSON文件描述了我们的程序以及依赖，比如添加mongodb库，内容如下

```json
"dependencies": {
  "body-parser": "~1.16.0",
  "cookie-parser": "~1.4.3",
  "debug": "~2.6.0",
  "express": "~4.14.1",
  "jade": "~1.11.0",
  "morgan": "~1.7.0",
  "serve-favicon": "~2.3.2",
  "mongodb": "~2.2.31"
}
```

Note：如何使用npm 查询库信息

```shell
npm view mongodb version  //mongodb库最新版本
npm view mongodb versions //mongodb库所有版本号
npm info mongodb		  //mongodb库详细信息
```



二、Mongodb创建数据库并读取数据

下载地址：http://www.mongodb.org/downloads

```shell
brew install mongodb
mongod -version //验证是否安装了MongoDB
```



使用robomongo连接数据库

下载：https://robomongo.org/download

打开robomongo，创建`localhost:27017`连接并connect即可

[Node.js开发笔记-4:MAC安装 mongodb 以及可视化工具](http://www.jianshu.com/p/99a92e077660)



加入数据库链接依赖，monk和mongoose都可以，具体看个人喜好，我选择了mongoose来操作数据，因为对数据库的一些基本操作mongoose也不是很复杂，以后深入学习也有一定的基础。

https://codeday.me/bug/20170727/47313.html

monk轻量，简单。

mongoose定位在orm,相对复杂，功能更强。初学者建议使用monk，熟悉后可再用mongoose。



##### 三、mongoose安装与使用

```shell
npm install mongoose
//建议app的依赖程序放在package.json包里，然后执行npm install安装
```

1、创建一个db.js文件来连接数据库`

```javascript
/*引入mongoose模块*/
var mongoose = require('mongoose'),
DB_URL = 'mongodb://localhost:27017/mongoosesample';

/**
 * 连接
 */
//mongoose.connection.openUri('mongodb://localhost/test')
mongoose.connection.openUri(DB_URL)

/**
  * 连接成功
  */
mongoose.connection.on('connected', function () {
		console.log('Mongoose connection open to ' + DB_URL);
});

/**
 * 连接异常
 */
mongoose.connection.on('error',function (err) {
		console.log('Mongoose connection error: ' + err);
});

/**
 * 连接断开
 */
mongoose.connection.on('disconnected', function () {
		console.log('Mongoose connection disconnected');
});

//导出mongoose对象　
module.exports = mongoose;
```

更多监听事件 http://mongoosejs.com/docs/api.html#connection_Connection



验证是否连接成功

```
node db.js
```

输出`Mongoose connection open to mongodb://localhost:27017/mongoosesample`表示成功



2、创建一个含Schema的模型，命名为user.js

```shell
/**用户信息 */
var mongoose = require('./db.js')

//1、定义一个Schema，指定字段名和类型
var UserSchema = new mongoose.Schema({
  //用户账号-定义一个属性name，类型为String,required必须
   username: {
      type: String,
      required: true
    },
    userpwd: { // 密码
      type: String,
      required: true
  },
	logindate : { type: Date}              //最近登录时间
});

//2、将该Schema发布为Model
var userModel = mongoose.model('User',UserSchema);

// 暴露接口,导出模型提供外部js文件引用
module.exports = userModel;
```

参考：[Mongoose学习参考文档——基础篇](https://cnodejs.org/topic/504b4924e2b84515770103dd)



常用数据库操作

创建一个test.js文件做一些常用操作演示

```shell
// 1、引入`user`
var User = require("./user.js");

/**
 * 插入
 */
function insert() {

    var user = new User({
        username : 'Tracy McGrady',                 //用户账号
        userpwd: 'abcd',                            //密码
        userage: 37,                                //年龄
        logindate : new Date()                      //最近登录时间
    });

    user.save(function (err, res) {

        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }

    });
}

insert();
```



其它操作

```shell
function update(){
    var wherestr = {'username' : 'Tracy McGrady'};
    var updatestr = {'userpwd': 'zzzz'};

    User.update(wherestr, updatestr, function(err, res){
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}

function findByIdAndUpdate(){
    var id = '56f2558b2dd74855a345edb2';
    var updatestr = {'userpwd': 'abcd'};

    User.findByIdAndUpdate(id, updatestr, function(err, res){
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}


function del(){
    var wherestr = {'username' : 'Tracy McGrady'};

    User.remove(wherestr, function(err, res){
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}

//update(); 更新
//findByIdAndUpdate(); 根据_id更新
//del(); 删除
```

基本流程

- 1、引入数据库连接，保证mongodb已经连接成功
- 2、引入模型（model）定义文件，即文档（表）结构定义
- 3、实例化UserModel，创建user实体
- 4、最后通过user实体对数据库进行操作，完成用户注册功能。

http://www.cnblogs.com/zhongweiv/p/mongoose.html

https://i5ting.github.io/wechat-dev-with-nodejs/db/mongoose.html



**nodejs建立socket.io服务**

通过nodejs的http模块就可以方便的搭建websocket服务器环境，例如下面的代码：

#### 安装socket.io

```shell
npm install socket.io
```

官方示例https://socket.io/docs/server-api/

socket.io+express多房间聊天应用
http://www.jianshu.com/p/40d8bc17529f
[利用socket.io+nodejs打造简单聊天室](http://www.jianshu.com/p/4ea6fc68f5f8)
[第十二天 长连接(net和socket.io)](http://www.jianshu.com/p/6966f12284b5)
[在Express和Socket.IO中使用session](http://js8.in/2011/09/29/%E5%9C%A8express%E5%92%8Csocket-io%E4%B8%AD%E4%BD%BF%E7%94%A8session/)
[基于Socket.IO 的私聊](http://www.moye.me/2015/01/02/node_socket-io/)
https://github.com/fengli12321/Socket.io-FLSocketIM-iOS
https://github.com/HOWIE-CH/-You-guess-I-painted-_socket
SRSocket
https://github.com/TheBloodElf/SocketIOChatDemo
https://github.com/jingtao910429/PomeloMessageCenterSocketIO
https://github.com/full-of-fire/Socket.IO_ChatDemo
https://github.com/saturngod/Socket.io-with-iOS

