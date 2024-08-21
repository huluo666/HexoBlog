---
title: Mac安装node.js
date: 2016-04-28 18:24:12
tags: iOS
grammar_cjkRuby: true
---

#### Mac 下安装node.js

http://www.jianshu.com/p/f21fdbdf47df

确保mac电脑上已经安装`homeBrew`，使用下面命令安装`node.js`

`brew install node`



#### MAC node.js升级

`node.js`升级步骤

第一步，先查看本机`node.js`版本：
`$ node -v`

```
第二步，清除node.js的cache：
   $ sudo npm cache clean -f

第三步，安装 n 工具，这个工具是专门用来管理node.js版本的，别怀疑这个工具的名字，是他是他就是他，他的名字就是 "n"
    $ sudo npm install -g n

第四步，安装最新版本的node.js
    $ sudo n stable

第五步，再次查看本机的node.js版本：
    $ node -v
```



## mac 卸载node

brew 安装的node ，命令卸载

`brew uninstall --force node`

