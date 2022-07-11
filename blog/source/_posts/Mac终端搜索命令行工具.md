---
title: Mac终端搜索命令行工具
date: 2017-12-29 14:54:42
tags:
categorys:
---

`mdfind`命令就是`Spotlight`功能的终端界面

```
mdfind -name "Photo 1.PNG"  //搜索文件名Photo 1.PNG
mdfind -onlyin ~/Library plist //搜索指定文件夹
```



### The Silver Searcher

https://github.com/ggreer/the_silver_searcher

```
brew install the_silver_searcher //安装
```

使用

```
Usage: ag [FILE-TYPE] [OPTIONS] PATTERN [PATH]
	   ag [文件类型]  [选择] [正则] []
```



常用命令行

```
常用参数
-i 忽略大小写
-l 只列出文件名
-g 文件名匹配
--php 只搜索php文件
--ignore-dir 忽略目录
```



```
ag  动画   			//搜索当前路径带文件名或文件内容带“动画”文件
ag -g 动画  		 	//搜索当前路径文件名带“动画”文件
ag "动画" './'        //搜索该目录下以及其子目录下的所有含有"image"的文件
ag --markdown  动画  //搜索指定类型
ag --ignore-dir sitedata --php hx /www/baidu.com
```



  -c --count              Only print the number of matches in each file.

匹配文件关键词个数

 -g PATTERN              Print filenames matching PATTERN

匹配文件

-l --files-with-matches Only print filenames that contain matches

只输出匹配内容//取相反结果 用大写-L

  -o --only-matching      Prints only the matching part of the lines

输出匹配所在行

