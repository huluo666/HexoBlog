---
title: Markdown语法学习备忘
tags:
categories: 
date: 2017-02-28 15:32:52
grammar_cjkRuby: true
---

来源于以下文章

[献给写作者的 Markdown 新手指南](http://www.jianshu.com/p/q81RER)

[Markdown 语法手册 （完整整理版）](http://blog.leanote.com/post/freewalk/Markdown-%E8%AF%AD%E6%B3%95%E6%89%8B%E5%86%8C)

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

1. 斜体和粗体

```markdown
*斜体*或_斜体_
**粗体**
***加粗斜体***
~~删除线~~
```

**链接和图片**

```markdown
[简书](http://www.jianshu.com)
![图片](http://7xr7vj.com1.z0.glb.clouddn.com/tmp268b5eed.png)
http://example.com/
<http://example.com/>
```

```markdown
我经常去的几个网站[Google][1]、[Leanote][2]以及[自己的博客][3]
[Leanote 笔记][2]是一个不错的[网站][]。
[1]:http://www.google.com "Google"
[2]:http://www.leanote.com "Leanote"
[3]:http://http://blog.leanote.com/freewalk "梵居闹市"
[网站]:http://http://blog.leanote.com/freewalk
```



列表

```markdown
- 无序列表项 一
- 无序列表项 二
- 无序列表项 三
```

引用

```markdown
>>> 请问 Markdwon 怎么用？ - 小白
>> 自己看教程！ - 愤青
> 教程在哪？ - 小白
```



### 首行缩进

写文章时，我们常常希望能够**首行缩进**，这时可以在段首加入`&ensp;`来输入一个空格.加入`&emsp;`来输入两个空格。

### 限制图片大小并居中

```markdown
<img src="http://7xr7vj.com1.z0.glb.clouddn.com/tmp268b5eed.png" width="400" height="400" alt="效果图"/>
```