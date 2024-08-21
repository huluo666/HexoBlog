---
title: hexo 博客搭建
date: 2016-02-01 10:33:55
tags: hexo
categories: hexo
grammar\_cjkRuby: true
---

### 一、hexo与相关环境安装
**1）、安装`node.js`，`git`环境**

```shell
brew install node  #使用homebrew安装，这个Mac使用者必备吧
brew install git   #如果安装了xcode，那么也就已经安装了git
# brew uninstall node 卸载node
```

查看node安装是否成功

```shell
node -v #输出版本信息 v6.x.x
```

**2）、安装hexo**

hexo的安装使用最好查看[hexo官网][1]，官网很详细，因为版本升级，网上那些信息可能不准确而多走弯路

```shell
$sudo npm install hexo-cli -g #全局安装Hexo客户端 3.0后
```

升级

```shell
$ hexo v                      #查看版本信息，检查是否安装成功
$ npm update hexo-cli -g      #更新hexo到最新版 
```

### 二、hexo初始化

```shell
$ cd xxx.github.io      #博客目录
$ hexo init  folder     #folder可不填
```

如果指定 `folder`，便会在目前的资料夹建立一个名为 `folder` 的新资料夹；否则会在目前资料夹初始化。

**相关依赖库安装**

本人发现安装 `hexo-cli`时已安装以下库，如果出现相应错误，则需要安装。

```shell
npm install  #这个命令会把需要的依赖环境都自动装上
#hexo从 3.0 开始把服务器独立成了个别模块，所以要本地调试的话，还需要安装 hexo-server 才能使用
npm install hexo-deployer-git --save  #如果没hexo d 失败则安装
npm install hexo-server --save        #如果没hexo s 失败则安装
```



### **三、创建新博客**

在当前目录`xxx.github.io`

`hexo new "Hello World"`



### 四、生成网站&本地调试

```shell
hexo clean    #清除缓存文件 (db.json) 和已生成的静态文件 (public)。
hexo generate #生成网站静态文件  可缩写为 hexo g
hexo server   #启动服务，     可缩写为 hexo s 
hexo deploy   #部署网站 部署前，需要预先生成静态文件 可缩写为 hexo d
```

常用组合命令

```shell
$ hexo d -g #生成部署
$ hexo s -g #生成预览
```

本地查看调试地址：http://localhost:4000/



也可以指定端口如`hexo s -p 80`

出现`Branch master set up to track remote branch master from https://github.com/xxxx.github.io.git.`表示部署成功

访问博客: [http://username.github.io][2]

- Q：卸载Hexo？
  A：3.0.0版本执行`$ npm uninstall hexo-cli -g`，之前版本执行`$ npm uninstall hexo -g`。

  ​

## 常见错误

**（1）、Cannot find module**

```
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }   
{ [Error: Cannot find module './build/Debug/  DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }   
```
这个问题烦了好久，最终找到解决办法。
$ npm install hexo --no-optional



**（2）、部署不成功**

`nothing to commit, working directory clean`
解决：3步

```
1.rm -rf .deploy
2.hexo generater
3.hexo deploy
```


建议用safari浏览器打开查看

**（3）、修改config.xml文件**

`YAMLException: can not read a block mapping entry; a multiline key may not be an implicit key at line 11, column 9:`

检查一下是不是 冒号后面没有写空格？



## 参考资料

https://hexo.io/zh-cn/
MAC 上 github + hexo 搭建博客教程

http://www.jianshu.com/p/fd878edb95e7

Homebrew简介及安装
http://www.cnblogs.com/lzrabbit/p/4032515.html
Homebrew官网 http://brew.sh/index\_zh-cn.html

Hexo安装和配置
MAC 上 github + hexo 搭建博客教程

http://www.lovebxm.com/2017/05/30/buildBlog/

http://www.jianshu.com/p/b7886271e21a
http://segmentfault.com/q/1010000002565189

http://xiaopingblog.cn/2016/04/08/untitled-1460084538799/

[手把手教你使用Hexo + Github Pages搭建个人独立博客][3]

https://dreajay.github.io/2014/11/24/hexo+github%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E7%B3%BB%E7%BB%9F/

域名绑定

[GitHub Pages 绑定来自阿里云的域名][4]

[1]:	https://hexo.io/zh-cn/
[2]:	http://username.github.io/
[3]:	https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/
[4]:	http://quantumman.me/blog/setting-up-a-domain-with-gitHub-pages.html