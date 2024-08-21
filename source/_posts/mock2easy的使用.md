---
title: mock2easy的使用
tags: node.js
categories: node.js
date: 2017-04-11 17:35:54
grammar_cjkRuby: true
---

​	开发时一直使用node.js来模拟API，都是手写js进行接口模拟，一直有个想法可以在网页上进行操作，鼠标简单点几下就模拟一个API,奈何对html，css等不是很熟悉，而mock2easy是非常方便的可视化模拟API工具。

https://www.npmjs.com/package/mock2easy

该项目作者也是来自阿里的开发人员，和淘宝RAP功能类似，相比淘宝rap比较卡，mock2easy更适合个人模拟开发测试。

#### 1、安装 `Grunt-cli`

```shell
$ npm install -g grunt-cli #全局安装
```

#### 2、生成 package.json 文件

```shell
$ npm init 	#输入name后可以一直回车
```

#### 3、安装mock2easy依赖插件

```shell
npm install --save-dev grunt connect-livereload grunt-contrib-connect grunt-contrib-watch grunt-mock2easy 
# npm install  --save-dev 插件1 插件2 插件3
```

官网demo很久没更新了，比如mock.js旧版很多方法没有，所以插件建议安装最新版。

#### 4、编辑Gruntfile.js文件

在`package.json`的同级目录新建Gruntfile.js文件，内容如下：

```
var serveStatic = require('serve-static');
module.exports = function (grunt) {
  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    watch: {
      options: {
        livereload: true
      },
    },
    connect: {
      debug: {
        options: {
          port: 3000,
          hostname: 'localhost',
          middleware: function (connect) {
            return [
              serveStatic('app'),
              require('./database/do')
            ];
          }
        }
      }
    },
    mock2easy: {
      dev:{
        options: {
          port:4000,
          lazyLoadTime:3000,
          database:'database',
          doc:'doc',
          keepAlive:false,
          ignoreField:['__preventCache'],
          interfaceSuffix:'.json',
          preferredLanguage:'cn'
        }
      }
    }
  });

  // Load the plugin that provides the "uglify" task.
  grunt.loadNpmTasks('grunt-contrib-connect');
  grunt.loadNpmTasks('grunt-mock2easy');
  grunt.loadNpmTasks('grunt-contrib-watch');

  // Default task(s).
  grunt.registerTask('default', ['mock2easy','connect','watch']);

};
```

然后终端输入`grunt`启动服务。

相关文章

[Grunt使用实例](http://www.jianshu.com/p/7d1cebeffdd8)

[Gruntfile.js配置全攻略](http://www.jianshu.com/p/78d556cd621c)