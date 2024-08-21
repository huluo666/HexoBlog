---
title: Xcode扩展开发之node.js/python读取本地文件
date: 2018-03-07 11:55:51
tags:
categorys:
---

最近在开发iOS开发辅助工具，其中包含Xcode扩展属性自动生成功能，属性生成模板代码以本地JSON文件的形式存在，也就是要把含有`\n`,`\t`的代码文本保存到JSON文件中，这样就保持原本的代码格式，用时直接插入即可。当然少量代码手工添加\n等格式符号也行，当大量代码显然不合适。

如何打印如下代码格式

```objectivec
- (NSString *)name {\n    if (!_name) {\n        _name = [[NSString alloc] init];\n    }\n    return _name;\n}\n\n@end\n'
```

方式一、iOS方式

使用数组方式打印，数组还包含\n,\t等格式符

步骤1、读取模板文件内容   2、分割字符串

```objectivec
NSString * templatePath = [[NSBundle mainBundle] pathForResource:templteName
                                                    ofType:nil];
NSString * templateContent= [NSString stringWithContentsOfFile:templatePath
                                              encoding:NSUTF8StringEncoding
                                                 error:nil];
NSArray *array=[autoCode componentsSeparatedByString:@"任意分割字符"];
NSLog(@"模板=%@",array);
```

但iOS启动慢，每次修改文件都要重新启动，为了高效率咱们使用

方式二、node.js

```js
var fs = require('fs');
var fileContent;
fs.readFile("~/iOSDev/XcodePlugIn/XcodePlugIn/ViewController.m", function(err,data)
	{
		if(err)
			console.log(err)
		else
		fileContent=data.toString();
		var string = fileContent;
		var array = [];
		array = string.split("Documents");
		console.log(array);

	});
```

[node.js读取本地文件](https://www.zybuluo.com/langlibaitiao/note/719829)

方式三、python

python就更简单了

```shell
afile = open('/Users/pconline/iOSDev/XcodePlugIn/XcodePlugIn/ViewController.m', 'r') # 打开文件
data=afile.read()
print(repr(data))

或
print(data.encode("string_escape").encode("utf-8"))
print(data.encode("unicode_escape").decode("utf-8"))
```

使用 [`repr`](https://docs.python.org/3/library/functions.html#repr)
https://stackoverflow.com/questions/21672334/javascript-how-to-show-escape-characters-in-a-string







