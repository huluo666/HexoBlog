```
title: JavaScript文件处理
date: 2022-07-21 16:28:48
categories:
tags:
```



```
var url="/Users/jenkins/Library/LaunchAgents/com.autotask.default.plist"
var path = url.substring(url.lastIndexOf('/')+1);
console.log("fileName:"+extension)

//var path="文件名.ppt"
var index=path.lastIndexOf(".");
var extension=path.substring(index+1,path.length);//index是点的位置。点的位置加1再到结尾
console.log("extension:"+extension)
var name=path.substring(0,index);
console.log("name:"+name)
```

