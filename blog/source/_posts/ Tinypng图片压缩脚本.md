---
title: Tinypng图片压缩脚本
date: 2018-01-08 14:36:37
tags:
categorys: python
---

本想写个Finder插件，鼠标右键弹出tinypng压缩菜单，直接点击压缩，在App程序上执行没问题，但Finder扩展上报错了，查了很多资料发现这是苹果的一个bug，哪怕开了沙盒，扩展也无法读写本地文件，几年了也没修复，所以放弃这种做法，脚本还是能用的。

https://tinypng.com/

##### 1、tinify安装

```
pip install --upgrade tinify
```

##### 2、将脚本与压缩图片放在同一目录执行脚本

```
cd xxxx/tinypng.py
./tinypng.py
```

tinypng.py

```python
#!/usr/bin/env python
# -- coding: UTF-8 --
import sys
import tinify
import os
import os.path

tinify.key = "92xubC2QKkKYUoeaD4yax-gAbER06fbh" #自己去申请tinify的开发者key，网址：https://tinypng.com/developers
#获取当前目录
#currentDir ="/Users/pconline/iOSDev/AllDevFile/python/test" # source path
currentDir = os.getcwd()
toFilePath = currentDir+"/tinyPng"         # 输出路径
print "currentDir=%s" %currentDir
print "toFilePath=%s" %toFilePath
#currentDir ="/Users/pconline/iOSDev/AllDevFile/python/test" # source path
#压缩的图片类型
supportImgType = ['.jpg','.png'];
#遍历目录下的图片，并批量压缩图片
for item in os.listdir(currentDir):
    itemPath=os.path.join(currentDir,item)
    if os.path.isfile(itemPath):
        print "文件名=="+item
        fileName, fileSuffix = os.path.splitext(item)
        if fileSuffix in supportImgType:
            #如果tinypng目录不存在就创建
            if os.path.isdir(toFilePath):
                pass
            else:
                os.mkdir(toFilePath)
            print('Doing:'+item) #当前正在压缩的图片名称
            toFullName = toFilePath + '/' + item
            source = tinify.from_file(itemPath)
            source.to_file(toFullName)
            print('Finnish:'+item) #压缩完成的图片名称
```

