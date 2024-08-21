---
title: iOS Model保存
date: 2017-10-10 18:24:12
tags: iOS
categories: iOS
grammar_cjkRuby: true
---

iOS Model保存

```objectivec
  -(void)saveUserRequestData
{
    _lastTreeModel=self.treeModel;
    NSUserDefaults * defaults = [NSUserDefaults standardUserDefaults];
    //不能直接存取NSObject，需要先归档转成NSData
    NSData * data  = [NSKeyedArchiver archivedDataWithRootObject:self.treeModel];
    [defaults setObject:data forKey:@"treeModel"];
}

-(void)getUserRequestData
{
    NSData * data = [[NSUserDefaults standardUserDefaults]objectForKey:@"treeModel"];
    //在这里解档
    RequestModel *treeModel = [NSKeyedUnarchiver unarchiveObjectWithData:data];
    self.treeModel=treeModel;
}
```