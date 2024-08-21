---
title: iOS富文本高亮
date: 2017-11-04 15:13:38
tags: iOS
categories: [iOS]
grammar_cjkRuby: true
---

多个predicates

use `NSCompoundPredicate` for your multiple predicates, you can refer [NSCompoundPredicate Class Reference](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/NSCompoundPredicate_Class/Reference/Reference.html)

[Use NSPredicate to search multiple fields](https://stackoverflow.com/questions/29043149/use-nspredicate-to-search-multiple-fields)

[NSPredicate endswith multiple files](https://stackoverflow.com/questions/5032541/nspredicate-endswith-multiple-files)

| NSStringEnumerationOptions               | 说明      |
| ---------------------------------------- | ------- |
| NSStringEnumerationByLines               | 按行      |
| NSStringEnumerationByParagraphs          | 段落      |
| NSStringEnumerationByComposedCharacterSequences | 字符顺序    |
| NSStringEnumerationByWords               | 单词，字    |
| NSStringEnumerationBySentences           | 句子      |
| NSStringEnumerationReverse               | 反向遍历    |
| NSStringEnumerationSubstringNotRequired  | 不需要子字符串 |
| NSStringEnumerationLocalized             |         |



```objectivec
-(void)processEditing
{
    NSRange paragaphRange = [self.string paragraphRangeForRange:self.editedRange];
    NSArray *extensions = [NSArray arrayWithObjects:@"strong", @"copy", @"assign", @"nonatomic", nil];
    NSPredicate *predicateWords =[NSPredicate predicateWithFormat:@"SELF IN %@", extensions];
    // 创建谓词对象并设定条件的表达式
    //    NSPredicate *predicateNS = [NSPredicate predicateWithFormat:@"SELF MATCHES '%@'",kATPattern];
    NSPredicate *predicateWD = [NSCompoundPredicate orPredicateWithSubpredicates:@[predicateWords]];

    [self.string enumerateSubstringsInRange:paragaphRange options:NSStringEnumerationByWords usingBlock:^(NSString * _Nullable substring, NSRange substringRange, NSRange enclosingRange, BOOL * _Nonnull stop) {
        NSLog(@"substring=%@",substring);
        if ([predicateWD evaluateWithObject:substring]){
            [self setAttributes:@{NSForegroundColorAttributeName:[NSColor redColor]} range:substringRange]; //当出现关键词如strong单词时字体变红
        }
    }];
}
```



多个匹配命令

```objectivec
// 按顺序获得所有（包括不匹配）字段的 range 数组以重新拼接
NSString *regex = [NSString stringWithFormat:@"%@|%@|%@|%@|%@|%@",kUserPattern, kCopyPattern, kNonatomicPattern, kNSClassPattern,kStrongPattern,kAssignPattern];
NSRegularExpression *expression = [NSRegularExpression regularExpressionWithPattern:regex options:NSRegularExpressionCaseInsensitive error:nil];
[expression enumerateMatchesInString:self.string options:0 range:paragaphRange usingBlock:^(NSTextCheckingResult *result, NSMatchingFlags flags, BOOL *stop) {
    // Add red highlight color
    [self addAttribute:NSForegroundColorAttributeName value:RGB(194, 53, 154) range:result.range];
}];
```

https://github.com/GGGHub/TextKitDemo/blob/master/TextKitDemo/TextKitDemo/LSYTextStorage.m

https://github.com/Desgard/Shanbei-Homework/blob/master/Shanbei%20Homework/HighlightingTextStorage.m

[iOS富文本（三）深入使用Text Kit](http://ggghub.com/2015/12/02/iOS%E5%AF%8C%E6%96%87%E6%9C%AC%EF%BC%88%E4%B8%89%EF%BC%89%E6%B7%B1%E5%85%A5%E4%BD%BF%E7%94%A8Text%20Kit/)

[Multiple NSPredicate](https://stackoverflow.com/questions/16642934/multiple-nspredicate)