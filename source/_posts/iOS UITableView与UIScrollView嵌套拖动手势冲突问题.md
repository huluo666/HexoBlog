---
title: iOS UITableView与UIScrollView嵌套拖动手势冲突问题
date: 2016-04-21 18:17:50
tags: iOS
grammar_cjkRuby: true
---

当 UITableView与UIScrollView嵌套使用时，会重现手势冲突问题

譬如：http://www.cocoachina.com/bbs/read.php?tid-241154.html

解决方案

```
tableView.scrollEnabled = NO;
set tableView.alwaysBounceVertical = NO;
```

[UITableView scrolling problems when inside a UIScrollView](http://stackoverflow.com/questions/17974154/uitableview-scrolling-problems-when-inside-a-uiscrollview)

