---
title: JavaScript正则匹配示例
date: 2018-03-05 12:08:40
tags:
categorys:
---



匹配文章id使用原生跳转

```javascript
var str = "http://bbs.pcauto.com.cn/topic-15113553.html";
var substr = str.match(/pcauto.com.cn\/topic-(\d*).html/);
console.log(substr);
console.log('匹配id='+substr[1]);
```



OC实现

```objectivec
NSRegularExpression *regex = [NSRegularExpression regularExpressionWithPattern:@"pcauto.com.cn/topic-(\\d*).html" options:NSRegularExpressionCaseInsensitive error:nil];

//匹配第一个
NSTextCheckingResult *result = [regex firstMatchInString:searchText options:0 range:NSMakeRange(0, [searchText length])];
NSRange matchRange = [result rangeAtIndex:1];
NSString *matchString = [searchText substringWithRange:matchRange];
NSLog(@"searchText=%@",matchString);

//匹配所有
NSArray* matches = [regex matchesInString:searchText options:0 range:NSMakeRange(0, [searchText length])];
if (matches.count != 0)
{
    NSLog(@"matches=%@",matches);
    for (NSTextCheckingResult *match in matches) {
        NSLog(@"match=%@", NSStringFromRange([match rangeAtIndex:2]));
        NSRange matchRange = [match rangeAtIndex:1];
        NSString *matchString = [searchText substringWithRange:matchRange];
        NSLog(@"matchString=%@", matchString);
    }
}
```

