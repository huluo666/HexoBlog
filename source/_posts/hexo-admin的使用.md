---
title: hexo-admin的使用
date: 2017-02-28 09:41:36
tags:
categorys:
---

[Hexo博客引擎](http://hexo.io/)的管理用户界面。基于 [Ghost](http://ghost.org/) 界面，灵感来自[svbtle](http://svbtle.com/)和[prose.io](http://prose.io/)。

本地使用vs部署

这个插件最初是作为本地编辑器设计的 - 您在本地运行hexo，用于`hexo-admin`创作帖子，然后使用`hexo generate`或`hexo deploy`将生成的静态HTML文件发送到github页面或其他静态服务器。

但是，`hexo-admin`只要您使用Heroku，DigitalOcean等非静态托管服务，就可以在您的实时博客上运行。静态托管服务（如Github页面和Surge.sh）不支持从以下位置运行hexo-admin：您的活网站。如果您使用的是直播博客中的Hexo管理员，则必须设置密码（请参阅下文） - 否则任何人都可以编辑您的内容。

![](https://github.com/jaredly/hexo-admin/raw/master/docs/pasted-0.png?raw=true)

![](https://github.com/jaredly/hexo-admin/raw/master/docs/pasted-1.png?raw=true)

## 快速开始

（首先搭建好博客）

### 1. 安装Hexo-Admin插件

```
cd yourblog   #(xxxx.github.io目录)
npm install --save hexo-admin  #安装Hexo-Admin插件

#部署 调试，打开编辑页
hexo server -d				
open http://localhost:4000/admin/
```

完成这步就可以在线编辑写文章发布了



2、设置密码，编辑Hexo`_config.yml`文件

打开 http://localhost:4000/admin/#/auth-setup

设置用户名，密码等

```
# hexo-admin authentification
admin:
  username: 
  password_hash:
  secret:
  deployCommand: 'deploy.cmd'
```

打开http://localhost:4000/admin/即可看到登录页面，第一次打开需要比较长的时间



3、在hexo博客目录下创建deploy.cmd文件。这个脚本的作用是渲染html、压缩html，css和js、部署文件到服务器端。

```
@echo off
call hexo g
call gulp
call hexo d
```



其它

升级版

https://github.com/xbotao/hexo-admin-qiniu