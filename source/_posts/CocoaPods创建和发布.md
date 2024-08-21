---
title: CocoaPods创建和发布
date: 2016-04-13 14:42:14
tags: CocoaPods
categories: iOS
grammar_cjkRuby: true
---

### 一、创建私有pod

**1.克隆项目到本地**

cd进入本地某个目录 ，从github上clone下项目

```
$ cd xxx/xxx/file
$ git clone https://github.com/huluo666/HJDefaultsCache.git
```

cd进入项目目录

```
$ cd xxx/xxx/HJDefaultsCache
```

**2.打tag**

本地项目是没有tag的，给项目加入一个tag。以便pod能自动识别

```
git tag 1.0.0  //git tag -m "注释" 0.0.2﻿
git push --tags
git push origin master
```



**3.创建podspec文件**

```sh
$ pod spec create  https://github.com/huluo666/HJDefaultsCache
```

编辑`.podspec`文件

```shell
Pod::Spec.new do |s|
  #项目名称
  s.name         = "HJDefaultsCache"
  #版本号
  s.version      = "1.0.0"			  //版本号
  #对开源项目的描述
  s.summary      = "为NSUserDefaults设置默认值"
  s.description  = <<-DESC
   					为NSUserDefaults设置默认值
                   DESC
  #开源项目的首页
  s.homepage     = "https://github.com/huluo666/HJDefaultsCache"
  #开源协议
  s.license = { :type => 'MIT', :text => <<-LICENSE
                   Copyright 2016
                   Permission is granted to...
                 LICENSE
               }
               
  s.author             = { "H罗" => "huluo666@126.com" }
  #适用于ios7及以上版本
  s.platform     = :ios, "7.0" 
  s.source       = { :git => "https://github.com/huluo666/HJDefaultsCache.git", :tag => "#{s.version}" }
  #源文件，这个就是供第三方使用的源文件
  s.source_files  = "Classes", "HJDefaultsCache/**/*.{h,m}"
  s.exclude_files = "Classes/Exclude"
  #使用的是ARC
  s.requires_arc = true
  # s.resources = "Resources/*.png"
  # s.dependency "JSONKit", "~> 1.4"
end
```

注意: 

- 1、s.version应和tag的版本一致.先push该文件之后,再push --tags


- 2、将源代码放置在固定的文件夹下,同时修改s.source


修改好的`podspec`文件记得上传同步

```shell
$ git add HJDefaultsCache.podspec
$ git commit -am "add HJDefaultsCache.podspec 注释”
$ git push -u origin master
```

最后，在你项目的Podfile里面加入这个第三方库的地址 `pod install`。

```shell
pod 'HJDefaultsCache', :git => 'https://github.com/huluo666/HJDefaultsCache.git'
```



### 二、使用Trunk创建官方CocoaPod

要想使用Trunk服务，首先你需要注册自己的电脑。这很简单，只要你指明你的邮箱地址（spec文件中的）和名称即可。

- 1.第一次使用,注册  `$ pod trunk register 793633193@qq.com 'huluo666'`

- 2.收到邮件激活后，检查是否注册成功 `pod trunk me`

ps:当然，如果你的pod是由多人维护的，你也可以添加其他维护者

```shell
$ pod trunk add-owner ARAnalytics kyle@cocoapods.org
```

- 3 .验证podspec文件是否有误 `pod spec lint Test.podspec`

     成功 `Test.podspec passed validation`.

- 4.push pod spec文件

  ```shell
    $ pod trunk push Test.podspec
  ```

等待部署成功。



### 三 、踩过的一些坑备忘

`-WARN  | [iOS] license: Unable to find a license file`

参考官网 https://guides.cocoapods.org/syntax/podspec.html#license

```wiki
 s.license = { :type => 'MIT', :text => <<-LICENSE
                   Copyright 2016
                   Permission is granted to...
                 LICENSE
               }
```

`WARN  | source: The version should be included in the Git tag.`

本地podspec文件和github上的tag要一致



其它命令

```
//删除Tag
git tag -d v1.0.0
git push origin :refs/tags/v1.0.0
```





### 大体流程

**1、创建描述文件**

`pod spec create  https://github.com/huluo666/NetWorkMonitorView`

- 若没有设置tag会有提示,执行下面命令，重新进行第一步即可

```shell
​```
$ git tag -a 1.0.0 -m "Tag release 1.0.0"
$ git push --tags
​```
```

**2.验证描述文件**

```shell
pod spec lint yourPodName.podspec
```

**3.trunk 发布**

```shell
pod trunk push  //如果有警告  pod trunk push --allow-warnings 
```



参考

https://guides.cocoapods.org/making/private-cocoapods.html

http://www.voidcn.com/blog/bluefish89/article/p-3673616.html

http://useyourloaf.com/blog/creating-a-cocoapod/

