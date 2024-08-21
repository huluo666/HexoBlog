---
title: html笔记
date: 2016-04-10 18:24:12
tags: html
categories: html
grammar_cjkRuby: true
---



html教程

http://www.runoob.com/html/html-tutorial.html

http://www.zb7.com/z/html/

http://w3school.com.cn/html/html_intro.asp



- <html> 与 </html> 之间的文本描述网页
- <head>与 </head> 这里是文档的头部 
- <body> 与 </body> 之间的文本是可见的页面内容（文档的主体）
- <h1> 与 </h1> 之间的文本被显示为标题
- <p> 与 </p> 之间的文本被显示为段落



| 标签                             | 描述          |
| ------------------------------ | ----------- |
| <html> 与 </html>               | 定义 HTML 文档。 |
| <body> 与 </body> 之             | 定义文档的主体。    |
| <h1> 与 </h1>                   | 定义 HTML 标题  |
| <hr>                           | 定义水平线。      |
| <!-->                          | 定义注释。       |
| <br />                         | 插入单个折行（换行）  |
| <a href="default.htm" >链接 </a> | 链接标签        |
|                                |             |



New : HTML5 中的新标签。

### 基础

| 标签           | 描述          |
| ------------ | ----------- |
| <!DOCTYPE>   | 定义文档类型。     |
| <html>       | 定义 HTML 文档。 |
| <title>      | 定义文档的标题。    |
| <body>       | 定义文档的主体。    |
| <h1> to <h6> | 定义 HTML 标题。 |
| <p>          | 定义段落。       |
| <br>         | 定义简单的折行。    |
| <hr>         | 定义水平线。      |
| <!--...-->   | 定义注释。       |



`标题` <h1> 定义最大的标题。<h6> 定义最小的标题。

`HTML 链接`由 <a> 标签定义。链接的地址在 `href` 属性中指定：

```html
<a href="http://www.w3school.com.cn">This is a link</a>
```

更多：http://w3school.com.cn/tags/html_ref_byfunc.asp



**HTML 的 style 属性**

应该避免使用下面这些标签和属性：在 HTML 4 中，有若干的标签和属性是被废弃的

| 标签                  | 描述          |
| ------------------- | ----------- |
| <center>            | 定义居中的内容。    |
| <font> 和 <basefont> | 定义 HTML 字体。 |
| <s> 和 <strike>      | 定义删除线文本     |
| <u>                 | 定义下划线文本     |
| 属性                  | 描述          |
| align               | 定义文本的对齐方式   |
| bgcolor             | 定义背景颜色      |
| color               | 定义文本颜色      |

对于以上这些标签和属性：请使用`样式`代替！



**HTML 颜色名**

HTML 4.0 标准仅支持 `16` 种*颜色名*，它们是：`aqua、black、blue、fuchsia、gray、green、lime、maroon、navy、olive、purple、red、silver、teal、white、yellow`。

PS：如果使用其它颜色的话，就应该使用*十六进制的颜色值*。

http://w3school.com.cn/tags/html_ref_colornames.asp



重要标签

`<!DOCTYPE>` 声明必须是 HTML 文档的第一行，位于 <html> 标签之前。

`<!DOCTYPE>` 声明`不是 HTML` 标签；它是指示 web 浏览器关于页面使用哪个 HTML 版本进行编写的指令。