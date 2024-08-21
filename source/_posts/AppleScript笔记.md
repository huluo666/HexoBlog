---
title: AppleScript笔记
date: 2016-10-09 11:24:12
tags: AppleScrips
categories: AppleScrips
grammar_cjkRuby: true
---

```shell

#获取输入框文本
set dialogString to  "请输入"
set returnedString to display dialog dialogString default answer ""
set yourInputText to the text returned of returnedString

# 打印log
display dialog returnedNumber
log returnedNumber

#字符串链接
set str1 to "hello"
set str2 to "world"
set str3 to str1 & ", " & str1
display dialog str3

#选择打开文件夹
set FolderPath to (choose folder) -- sets file path to folder you select
display dialog FolderPath as text
#tell application "Finder" to open FolderPath--打开文件
tell application "Finder" to reveal FolderPath--在文件夹中显示
```





**do shell script in AppleScript**

https://developer.apple.com/library/content/technotes/tn2065/_index.html



系列参考教程

[AppleScript学习笔记（一）初识AppleScript](http://blog.csdn.net/jymn_chen/article/details/19755895)





```
display dialog "请输入" default answer linefeed

display dialog "What is your name?" default answer ""
```





```
set dialogString to "Input a number here"

set returnedString to display dialog dialogString default answer ""

set returnedNumber to the text returned of returnedString




try

set returnedNumber to returnedNumber as number

set calNumber to returnedNumber * 100

﻿  display dialog calNumber

on error the error_message number the error_number

display dialog "Error:" & the error_number & " Details:" & the error_message

end try

beep

```