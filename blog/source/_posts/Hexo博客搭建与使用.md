---
title: Hexo博客搭建与使用
date: 2020-07-08 16:51:17
tags:
---



## Hexo官网

https://hexo.io/zh-cn/



## 安装hexo

```sh
npm install hexo-cli -g
# 初始化博客目录
hexo init Blog
# 进入博客目录
cd Blog
# 安装博客相关依赖
npm install
hexo -v #验证
```



### 创建文章

### 1.创建一个md文件

md文件也就是`Markdown`文件，通过以下命令来创建：

```sh
$ hexo new <title>
$ hexo new "我的第一篇文章"
```

### 2.布局（layout）

- 创建md文件时，我们可以指定布局

```sh
$ hexo new [layout] <title>
$ hexo new post "测试文章"
$ hexo new page "about" 
$ hexo new page about
新建一个标题为 about 的页面，默认链接地址为 主页地址/about/
$hexo new "新文章"
新建一个标题为 新文章 的页面，文章标题日期可以随时修改


➜  blog git:(master) ✗ hexo new post "测试文章"
INFO  Validating config
INFO  Created: ~/Gitee/myblog/blog/source/_posts/测试文章.md
```

- 布局有三种：`post`（文章）、`draft`（草稿）、`page`（页面）

Front-matter预定义参数

```
layout  布局  默认为true，如果你不想你的文章被处理，可以设置为false
title  标题  标题会显示在最上方居中位置     
date  建立日期    如果不指定则为默认值-文件创建日期，可以自定义。
update  更新日期  如果不指定则为默认值-文件修改后重新生成静态文件的日期。
comments  是否开启文章的评论功能 默认值为true
tags  标签（不适用于页面page布局）
categoreies  分类（不适用于页面page布局）
permalink  覆盖文章网址
keywords  仅用于 meta 标签和 Open Graph 的关键词（不推荐使用）
```

### 为文章添加多个分类

1）下面文章属于三个分类：日常 > 生活，日常 > 随想，日记
2）其中生活、随想为日常的子分类，日常和日记为同级分类

```
categories:
- [日常, 生活]
- [日常, 随想]
- [日记]
```

**一般发布文章或者修改博客后需要这些操作：**清除缓存>生成静态文件>启动服务器，测试没问题后再部署。

```
// 我们可以写成一条命令
$ hexo clean && hexo g && hexo s
$ hexo d
```



### 文章头示例

```yaml
---
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
author: 赵奇
img: /source/images/xxx.jpg
top: true
hide: false
cover: true
coverImg: /images/1.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: 这是你自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要
categories: Markdown
tags:
  - Typora
  - Markdown
  
description：摘要内容 #如果主题配置项中的 excerpt_description：true 则摘要会在首页作为本文的全部内容显示出，通过点击标题或更多看完整的文章内容。
tags: #多个标签用[]扩起来,标签中间用半角逗号","分割、tags关键字自动生成（这里的tags就是/source/tags/index.md中配置的type: tags）
  
---
```



```
title: 全栈开发实战：用Vue2+Koa1开发完整的前后端项目（更新Koa2）
tags: 
  - 前端
  - Nodejs
categories:
  - Web
  - 开发
  - Nodejs
date: 2017-05-03 14:09:00
---
```



```
title: 使用Hexo创建文章、标签和分类
date: 2022-07-11 17:39:44
categories: [iOS, Python]
```



### 插件

https://hexo.io/plugins/

```
hexo clean && hexo g && hexo s
```



### Gitee自动化部署gitee pages

Gitee Pages 的访问速度很快，但无法GitHub Pages那样自动更新Pages，因为 Gitee 的自动部署属于 Gitee Pages Pro 的服务。

详细操作:https://github.com/marketplace/actions/gitee-pages-action

GitHub Actions：https://docs.github.com/cn/actions

## 前提

### 创建所需仓库

1. 创建 `blog` 仓库用来存放 Hexo 项目
2. 创建 `your.github.io` 仓库用来存放静态博客页面





## 创建工作流配置











参考文章

https://juejin.cn/post/7011765438262558727

https://sanonz.github.io/2020/deploy-a-hexo-blog-from-github-actions/
