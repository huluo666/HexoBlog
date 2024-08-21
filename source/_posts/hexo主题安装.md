---
title: hexo主题安装
date: 2016-02-02 10:33:55
tags: hexo
categories: hexo
grammar_cjkRuby: true
---

首先将hexo主题先克隆到自己电脑上：

进入hexo 安装目录，也就是初始化的目录（hexo init githexo）中的githexo目录，详细目录请根据自己定义的目录名字为主，

- `cd huluo666.github.io`    站点文件目录
- `git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant`。

克隆完毕`themes`目录会出现`maupassant`主题目录

- **接下来一定要输入下面的命令，来安装这个主题的依赖环境。**

```shell
npm install hexo-renderer-sass --save
npm install hexo-renderer-jade --save
```

由于墙等不定因素可能导致安装错误

可以先安装cnpm:

```
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
```

然后使用cnpm安装：

```
sudo cnpm install hexo-renderer-jade --save
sudo cnpm install hexo-renderer-sass --save
```

[给电脑换源 npm 国内镜像 cnpm](http://yijiebuyi.com/blog/b12eac891cdc5f0dff127ae18dc386d4.html)



#### 安装主题

接下来我们只需要将站点下的`_config.yml`配置文件中找到`theme：xxx`行主题配置更换成`theme：maupassant`

其实这样主题就配好了，其它的图标之类的可以根据需要修改即可。

**然后输入`hexo s` 或者 `hexo server` 就可以查看更换后的主题效果了

http://localhost:4000/



文章头部样式

```
---
title: ChangedBlogThemes
toc: true   // 文章侧边增加文章目录
date: 2016-11-23 21:16:14
tags: Themes
categories: Hexo
---
```



将npm 的镜像换成了阿里的 然后重新按照步骤执行了一下 就好了

```
npm install node-sass --registry=http://registry.npm.taobao.org --sass-binary-site=http://npm.taobao.org/mirrors/node-sass
```



### Maupassant主题安装

主题地址：<https://github.com/tufu9441/maupassant-hexo>

##### 1、安装主题和渲染器

```shell
cd 'hexo博客目录'
git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
npm install hexo-renderer-jade --save // 渲染器
npm install hexo-renderer-sass --save
npm install hexo-generator-feed --save // rss支持
```

##### 2、设置hexo主题

编辑hexo的_config.yml，设置主题名和语言

```js
theme: maupassant   # 渲染主题名
language: zh-CN 	# 默认英文
```

##### 3、搜索

添加站内搜索

```shell
npm install hexo-generator-search --save  #安装hexo搜索插件
```

然后，编辑theme/maupassant目录下的_config.yml，分hexo配置文件

```
// 添加字段
search:
  - path: search.xml
  - field: post
```

4、语法高亮



maupassant换图标教程：http://wdxtub.com/2016/03/05/maupassant-icon-config/