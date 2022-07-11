---
title: Mac 终端下运行shell/applescript等脚本Permission denied问题
date: 2016-11-18 19:14:12
tags: shell
categories: [shell]
grammar_cjkRuby: true
---

**运行脚本**

1、写好自己的 脚本，比如aa.sh 

2、打开终端 执行，方法一： 输入命令    ./aa.sh     ,方法二：直接把 aa.sh 拖入到终端里面。

**Permission denied问题**

修改该文件aa.sh 的权限 ：使用命令： `chmod 777 aa.sh` 。

或`sudo chmod 755 'filename'`   `chmod a+x ./'filename'`

然后再执行 上面第二步的操作 就 OK .



**批量修改**

如果有N多个文件，或者文件夹，如何批量修改呢？答案是使用chmod -R 777  [FolderName]（中括号里是你的文件夹名，实际输入不包括中括号