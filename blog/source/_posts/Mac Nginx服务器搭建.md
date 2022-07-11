---
title: Mac Nginx服务器搭建
date: 2017-02-03 08:24:12
tags: iOS
grammar_cjkRuby: true
---

**1.使用`Homebrew`安装`Nginx**`

```shell
brew install nginx
```



出现`mkdir: /usr/local/var/log/nginx: Permission denied`问题

```
sudo chown -R "$USER":admin /usr/local
或
sudo chown -R "$USER":admin /Library/Caches/Homebrew
```

http://stackoverflow.com/questions/16432071/how-to-fix-homebrew-permissions



**2.启动Nginx**

```
nginx
```

然后在浏览器里输入`localhost:8080`，回车，出现`welcome to nginx`，说明成功

#### 

#### 相关

**安装Homebrew**

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```