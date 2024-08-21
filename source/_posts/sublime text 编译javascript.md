---
title: sublime text 编译javascript
date: 2016-05-9 15:28:29
tags: iOS
grammar_cjkRuby: true
---



**使用Brew安装**

安装Node and NPM `brew uninstall node`

升级  `brew upgrade node`



### 一、添加node.js的build system设置方法 

打开 Tools -> Build System -> New Build System 
新建 node.sublime-build 编译系统配置文件，内容如下

```
{
  "cmd": ["/usr/local/bin/node", "$file"],
  "selector": "source.js"
}
```

保存为，node.sublime-build 
以后，写完node.js测试代码后，可以直接 Command + B 运行代码



如果需要运行ES6

参考：[Sublime Text内运行javascript(ES6)](https://segmentfault.com/a/1190000002291126)

```shell
{
    "cmd": ["/usr/local/bin/node", "--use-strict", "--harmony", "$file"],
    "selector": "source.js"
}
```



### 二、系统

注意用`debug()`替换`console.log()`，可支持到es5。

```
 {
 "cmd": ["jsc", "$file"],
 "selector": "source.js"
 }
```





编译配置文件路径

```shell
$ cd /Users/pconline/Library/Application\ Support/Sublime\ Text\ 3/Packages/User
```



[How to Create a Javascript Console in Sublime Text](http://www.wikihow.com/Create-a-Javascript-Console-in-Sublime-Text)

https://blog.netsh.org/posts/sublime-text-javascript-console_1820.netsh.html#-jsc-javascript-mac-os-x



插件

http://www.cnblogs.com/hykun/p/sublimeText3.html

SublimeText-Nodejs







