---
title: Mac文件搜索
date: 2017-10-10 18:24:12
tags: iOS
categories: iOS
grammar_cjkRuby: true
---

#### Mac文件搜索工具
find any file
Easyfind


[Mac常用命令](http://www.jianshu.com/p/456003ed9df0)

**通过Find命令搜索文件**

```shell
格式：find path(搜索路径,为空时默认为当前目录) -option(搜索条件) [-print][-exec -ok command] {}\
简单来说:find <路径> <-条件> <结果处理>

常用搜索条件(不全)：
-name filename              #查找名为filename的文件
-user username              #查找属于username的文件
-group groupname            #查找属于username的文件
-mtime -n +n                #按文件更改时间来查找文件，-n指n天以内，+n指n天以前
-ctime -n +n                #按文件创建时间来查找文件，-n指n天以内，+n指n天以前
-newer f1 !f2               #查更改时间比f1新但比f2旧的文件
-type b/d/c/p/l/f           #查是块设备、目录、字符设备、管道、符号链接、普通文件
-size n[c]                  #查长度为n块[或n字节]的文件
-depth                      #使查找在进入子目录前先行查找完本目录
-mount                      #查文件时不跨越文件系统mount点
-follow                     #如果遇到符号链接文件，就跟踪链接所指的文件

如:
find /Users/DFei_He/desktop -name '*.html' 搜索桌面所有html文件
find .   -perm   755 查看目录下权限为755的文件
```

find命令非常高效，并且使用简单。find命令来自unix，OS X和Linux系统同样支持该命令。find最基本的操作就是：

```shell
find 文件路径 参数
```

比如你可以通过以下命令在用户文件夹中搜索名字中包含screen的文件

```shell
find ~ -iname  "screen*"
```

你也可以在特定的文件夹中寻找特定的文件，比如

```shell
find ~/Library/ -iname "com.apple.syncedpreferences.plist"
```

这个命令可以在Library文件夹中寻找com.apple.syncedpreferences.plist文件


**通过mdfind命令搜索文件**

mdfind命令就是Spotlight功能的终端界面，这意味着如果Spotlight被禁用，mdfind命令也将无法工作。mdfind命令非常迅速、高效。最基本的使用方法是：

```shell
mdfind -name 文件名字
```

比如你可以通过下面的命令寻找Photo 1.PNG文件

```shell
mdfind -name "Photo 1.PNG"
```

因为mdfind就是Spotlight功能的终端界面，你还可以使用mdfind寻找文件和文件夹的内容，比如通过以下命令寻找所有包含Will Pearson文字的文件：

```shell
mdfind "Will Pearson"
```

mdfind命令还可以通过-onlyin参数搜索特定文件夹的内容，比如

```shell
mdfind -onlyin ~/Library plist
```

这条命令可以搜索Library文件夹中所有plist文件

