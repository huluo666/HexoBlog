---
title: NSTextView使用TextKit进行语法高亮
date: 2018-03-14 16:10:08
tags:
categorys:
---

NStextView使用textkit用法与iOS 基本相同，使用参考obj.io相关文章
https://www.objc.io/issues/5-ios7/getting-to-know-textkit/
https://github.com/objcio/issue-5-textkit
Rendering Markdown with Syntax Highlighting
https://github.com/objcio/S01E91-rendering-markdown-with-syntax-highlighting


**Mac下使用Textkit存在的一些问题**

在对文字进行高亮时，如果用户在句中或句头进行编辑，鼠标位置会发生改变
解决参考方案
https://stackoverflow.com/questions/35522394/nstextstorage-syntax-markdown

[Why the Selection Changes When You Do Syntax Highlighting in a NSTextView and What You Can Do About It](http://christiantietze.de/posts/2017/11/syntax-highlight-nstextstorage-insertion-point-change/)



