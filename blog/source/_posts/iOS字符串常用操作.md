---
title: 字符串常用操作
date: 2018-01-11 12:36:31
tags:
categorys: iOS
---

#### ios 去除字符串首尾空格、换行

1）如果您只需要从字符串中删除给定的字符（比如空格字符），请使用：

```
NSString *yourString = @"   .@^this text has spaces before and after*& ";
[yourString stringByReplacingOccurrencesOfString:@" " withString:@""]
```

2）如果你真的需要删除一组字符（即不仅是空格字符，而是任何空格字符，如空格，制表符，牢不可破的空间等），你可以拆分你的字符串使用`whitespaceCharacterSet`，然后再加入单词在一个串：

```
NSArray* words = [yourString componentsSeparatedByCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
NSString* nospacestring = [words componentsJoinedByString:@""];
```



清除首尾空格(注意：NSCharacterSet`只会操作字符串的首尾`)

```objectivec
+ (NSCharacterSet *)controlCharacterSet;  //控制符
+ (NSCharacterSet *)whitespaceCharacterSet; //空格
+ (NSCharacterSet *)whitespaceAndNewlineCharacterSet; //空格和换行
+ (NSCharacterSet *)decimalDigitCharacterSet; //小数
+ (NSCharacterSet *)letterCharacterSet; //文字
+ (NSCharacterSet *)lowercaseLetterCharacterSet; //字母数字
+ (NSCharacterSet *)uppercaseLetterCharacterSet; //可分解
+ (NSCharacterSet *)nonBaseCharacterSet;
+ (NSCharacterSet *)alphanumericCharacterSet; //所有数字和字母(大小写)
+ (NSCharacterSet *)decomposableCharacterSet;////0-9的数字
+ (NSCharacterSet *)illegalCharacterSet; //非法
+ (NSCharacterSet *)punctuationCharacterSet; //标点符号
+ (NSCharacterSet *)capitalizedLetterCharacterSet; //大写
+ (NSCharacterSet *)symbolCharacterSet; //符号
+ (NSCharacterSet *)newlineCharacterSet NS_AVAILABLE(10_5, 2_0); //换行符
```

字符串操作

```objectivec
NSString *string = @"   .@^this text has spaces before and after*& ";

/*去除字符串中的空格*/
NSString *trimmedString = [string stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceCharacterSet]];

/*去除字符串中的空格和换行*/
trimmedString = [trimmedString stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];

/*去除字符串中的标点*/
trimmedString = [trimmedString stringByTrimmingCharactersInSet:[NSCharacterSet punctuationCharacterSet]];

/*去除字符串中的符号*/
trimmedString = [trimmedString stringByTrimmingCharactersInSet:[NSCharacterSet symbolCharacterSet]];


/*去除字符串中的所有空格符号*/
-(NSString*)removeAllWhiteSpace:(NSString*)original
{
    NSCharacterSet *whitespaces = [NSCharacterSet whitespaceAndNewlineCharacterSet];
    NSArray *parts = [original componentsSeparatedByCharactersInSet:whitespaces];
    NSString* rval = [parts componentsJoinedByString:@""];
    return rval;
}
```

[Remove all whitespaces from NSString](https://stackoverflow.com/questions/7628470/remove-all-whitespaces-from-nsstring)

https://www.zybuluo.com/chinese-ppmt/note/609656