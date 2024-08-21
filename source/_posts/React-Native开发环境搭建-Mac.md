---
title: React Native开发环境搭建-Mac
date: 2018-04-02 10:34:41
tags:
categorys:
---

#### 1、React Native Command Line Tools安装

```
npm install -g react-native-cli
```

如果你看到`EACCES: permission denied`这样的权限报错，那么请参照上文的homebrew译注，修复/usr/local目录的所有权

```
sudo chown -R `whoami` /usr/local
```

帮助

```
react-native --help
```



2、创建 HelloWorld ReactNative

```
react-native init <项目名字>
如：
react-native init HelloWorld
```



3、运行应用

```
cd HelloWorld
react-native run-ios
react-native run-android
```

或直接用xcode打开工程

执行react-native run-ios 遇到下面错误：

`xcrun: error: unable to find utility "instruments", not a developer tool or in PATH`

解决方法：在 终端执行如下命令

```
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer/
```

“react/rctbundleurlprovider.h no found”

原因是没有启动npm

```
cd HelloWorld
npm start
```

或者

```
1、删除node modules文件夹，然后重新执行npm install命令。
2、执行react-native upgrade 按照提示覆盖旧文件。
3、清楚xcode缓存shift+alt+command+k
```

有时出现错误，可能重启一下就ok了.

[ReactNative规范指南](https://github.com/JasonBoy/javascript/blob/master/react/README.md)

