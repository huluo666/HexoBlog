---
title: iOS使用js库实现css转换json
tags: JavaScript
categories: JavaScript
date: 2017-08-02 18:08:43
---

https://github.com/aramk/CSSJSON

在cssjson.js文件末尾加入css转换json然后json字符串输出函数

```javascript
function exampleCSSJSON(cssString) {
    var json = CSSJSON.toJSON(cssString);
    var jsonStr = JSON.stringify(json);
    return jsonStr;
}
```



使用

```objectivec
-(void)doCssParser
{
    JSContext *context = [[JSContext alloc] init];
    context.exceptionHandler = ^(JSContext *context, JSValue *exception) {
        NSLog(@"# JS Exception: %@", exception);
        context.exception = exception;
    };

    NSString *cssText = [NSString stringWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"agate" ofType:@"css"]
                                                  encoding:NSUTF8StringEncoding
                                                     error:nil];
    NSString *jsPath = [[NSBundle mainBundle] pathForResource:@"cssjson" ofType:@"js"];
    NSString *jsString = [NSString stringWithContentsOfFile:jsPath encoding:NSUTF8StringEncoding error:nil];
    [context evaluateScript:jsString];

    JSValue *function = context[@"exampleCSSJSON"];
    JSValue* result = [function callWithArguments:@[cssText]];
    NSString *jsonStr=[result toString];
    NSLog(@"result0:%@",jsonStr);
}

//处理相同一个value对应多个key情况
-(void)fomatFinalJSONDict:(NSDictionary*)jsonDict
 {
       NSDictionary *subDict=jsonDict[@"children"];
    NSMutableDictionary *mdict=[NSMutableDictionary dictionary];
    [subDict enumerateKeysAndObjectsUsingBlock:^(NSString *key, id  obj, BOOL * _Nonnull stop) {
        NSArray *keyArr=[key componentsSeparatedByString:@",\n"];
        NSLog(@"keyArr:%@",keyArr);
        if (keyArr.count>1) {
            for (NSString *subKey in keyArr) {
                [mdict setObject:obj?:@"" forKey:subKey];
            }
        }else{
            [mdict setObject:obj?:@"" forKey:key];
        }
    }];
 }
```





使用原生oc库解析

https://github.com/tracy-e/ESCssParser

```
#import "ESCssParser.h"
-(void)doESCssParser2
{
    NSString *cssText = [NSString stringWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"agate" ofType:@"css"]
                                                  encoding:NSUTF8StringEncoding
                                                     error:nil];
    ESCssParser *parser = [[ESCssParser alloc] init];
    NSDictionary *styleSheet = [parser parseText:cssText];
    NSLog(@"styleSheet: %@", styleSheet);
}
```

虽然使用ESCssParser代码相对较少，但ESCssParser库代码量是很大的，使用js库解析，适用多平台同规则解析，相对全能多了。



html演示

```html
<body>
<style>
    body {
        font-family: arial, sans-serif;
        font-size: 18px;
    }
    pre {
        background: #eee;
        padding: 10px;
        width: 800px;
        white-space: pre-wrap;
        word-break: break-all;
    }
</style>

<!-- Add this if you're supporting IE 7, 8 -->
<script type="text/javascript" src="cssjson.js"></script>
<script type="text/javascript">

    // Dummy CSS
    var css = 'test';
    var json =exampleCSSJSON(css);
	document.write('<strong>JSON</strong><br/><pre>'+json+'</pre>');
</script>
</body>

```
其它参考

http://blog.cnbang.net/tech/2630/