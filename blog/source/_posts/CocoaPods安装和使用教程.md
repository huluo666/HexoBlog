---
title: CocoaPods安装和使用教程
date: 2016-04-12 11:42:14
tags: CocoaPods
categories: iOS
grammar_cjkRuby: true
---

CocoaPods安装和使用教程 http://code4app.com/article/cocoapods-install-usage

code4App的安装教程已经非常详细了，但由于软件环境不同可能出现不同问题，补充几点。



1.安装Ruby环境淘宝源应该使用`https`

```
 $ gem sources -a https://ruby.taobao.org/
```



`问题解决`

```sh
ERROR:  While executing gem ... (Errno::EPERM)
    Operation not permitted - /usr/bin/pod
```

解决`sudo gem install -n /usr/local/bin cocoapods`  [stack flow](http://stackoverflow.com/questions/30812777/cannot-install-cocoa-pods-after-uninstalling-results-in-error)

OR

```
sudo gem install -n /usr/local/bin cocoapods --pre
```



执行 `pod setup`时感觉卡住不动，一直停留在`Setting up CocoaPods master repo`
这是要去把整个specs仓库clone下来，下载到 ~/.cocoapods里，文件较大，比较耗时，可以查看是否正在下载中...

```shell
$ cd  ~/.cocoapods		//进入cocoapods目录
$ du -sh *  			//查看文件大小
```



经过漫长等待终于`Setup completed`，文件大小约`667M`









