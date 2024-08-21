---
title: macOS 小技巧
date: 2018-02-02 15:17:51
tags:
categorys: MacOS
---

macOS 小技巧

#### 1、xxxApp已损坏，打不开，你应该将它移到废纸篓

更新到最新系统 macOS Sierra 后发现`系统偏好设置->安全性与隐私`中默认已经隐藏了运行安装任何来源App的选项

在终端中输入命令 

```shell
sudo spctl --master-disable  #全局
```

针对某一APP

```shell
sudo xattr -rd com.apple.quarantine /Applications/应用.app
```



#### 2、弹出Emoji表情

```
Shift+Control+E 
```

