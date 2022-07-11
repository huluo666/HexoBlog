---
title: Mac Homebrew使用
date: 2016-05-04 15:13:38
tags: 工具
categories: [工具]
grammar_cjkRuby: true
---

mac系统也是基于unix的系统，所以也继承类很多unix的特性，包括软件的编译，安装等。`ubuntu`下有快捷命令`apt-get install`来快速安装软件。centos下有`yum install`来快速安装。所以，mac下也有一种方式，就是使用`brew`。

官网：http://brew.sh/index_zh-cn.html  中文： http://brew.sh/



**一、安装**

终端键入一下命令

```shell
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"	
```

- 安装完成后最好执行 `brew update` 以保证Homebrew是最新版本。
- 运行 `brew doctor` 以确保系统已经能正常使用brew。


`brew -v` 	查看版本

`brew update`	升级，版本过低将会导致无法安装后续几个组件。



1、更新brew本身

```
brew update

```



2、使用brew安装软件

```
brew install soft_name
// soft_name为你所要安装软件的标志，如使用brew安装git
brew install git

```



3、使用brew卸载软件

```
brew uninstall soft_name
// soft_name为你所要卸载软件的标志，如使用brew卸载git
brew uninstall git

```



4、显示使用brew安装的软件列表

```
brew list

```



5、更新软件

```
brew upgrade        // 更新所有使用brew安装的软件
brew upgrade git    // 更新某个使用brew安装的软件
```



6、查看哪些软件需要更新

```
brew outdated
```



7、查找软件

```
// 当你记不清软件的名字的时候，你可以使用search，只需要写去几个字母，他就会帮你联想，并把所有可能的结果输出给你
brew search
```



[Mac下使用brew安装mongodb](http://hcysun.me/2015/11/21/Mac%E4%B8%8B%E4%BD%BF%E7%94%A8brew%E5%AE%89%E8%A3%85mongodb/)


https://www.zybuluo.com/phper/note/87055

http://ksmx.me/homebrew-cask-cli-workflow-to-install-mac-applications/

http://gnahceg.github.io/install-OSX.html