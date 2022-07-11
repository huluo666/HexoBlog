---
title: Mac开发路径选择
date: 2017-02-13 20:18:35
tags: iOS
grammar_cjkRuby: true
---

这条命令可以搜索Library文件夹中所有plist文件。
find 基本用法

常用的文件查找命令主要有locate和find。


`find path -name "(字符，可以用wildcard)"`

默认情况下搜寻`path`以及其所有子目录下的文件。

### 举例

`find` 文件路径 参数

```
find . -name "*蝙蝠侠*"
# 找出当前目录以及其所有子目录下所有名字中包含“蝙蝠侠”三字的文件

find . -name "*.rmvb" -maxdept 1
# 找出当前目录（不包括子目录）下所有名字中后缀为".rmvb"的文件

find ~ -iname  "screen*"
#在用户文件夹中搜索名字中包含screen的文件

# find /etc -name 'passwd'    ：表示匹配/etc目录下文件名为passwd的文件 
# find /etc -name 'passwd*'   ：表示匹配/etc目录下文件名中以passwd开头的文件 
# find /etc -name '*passwd'   ：表示匹配/etc目录下文件名中以passwd结尾的文件 
# find /etc -name '*passwd*'  ：表示匹配/etc目录下文件名中有passwd字符串的文件 
```





**通过mdfind命令搜索文件**

mdfind -name 文件名字

```
mdfind -name "Photo 1.PNG"
```



因为mdfind就是Spotlight功能的终端界面，你还可以使用mdfind寻找文件和文件夹的内容，比如通过以下命令寻找所有包含Will Pearson文字的文件：

```
mdfind "Will Pearson"
```



mdfind命令还可以通过-onlyin参数搜索特定文件夹的内容，比如

```
mdfind -onlyin ~/Library plist
```

这条命令可以搜索Library文件夹中所有plist文件。

搜索导出列表

sudo find / -name *** > Desktop/find.txt