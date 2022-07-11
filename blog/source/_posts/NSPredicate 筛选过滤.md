---
title: iOS谓词NSPredicate 筛选过滤
date: 2016-11-04 15:13:38
tags: iOS
categories: [iOS]
grammar_cjkRuby: true
---

NSPredicate 筛选过滤

基本用法

```
1.创建NSPredicate（相当于创建一个过滤条件）
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"过滤条件"];

2.判断指定的对象是否满足NSPredicate创建的过滤条件
[predicate evaluateWithObject:person];

3.过滤出符合条件的对象（返回所有符合条件的对象）
NSArray *persons = [array filteredArrayUsingPredicate:predicate];
```

`+predicateWithFormat:来实际创建谓词。`可以使用单引号，双引号需要进行转义



```objectivec
//基本的查询
NSPredicate *predicate;
//方法一：
predicate = [NSPredicate predicateWithFormat:@"name == 'Herbie'"];

//方法二：
predicate = [NSPredicate predicateWithFormat:@"name == %@", @"Herbie"];

//方法三：%K表示key
predicate = [NSPredicate predicateWithFormat:@"%K == %@", @"name", @"Herbie"];
BOOL match = [predicate evaluateWithObject:car];
NSLog(@"%s", (match) ? "YES" : "NO");


以上为对象属性匹配，如果数组中都是字符串如何匹配－－self
NSArray *array=[NSArray arrayWithObjects: @"abc", @"def", @"ghi",@"jkl", nil nil];
NSPredicate *pre = [NSPredicate predicateWithFormat:@"SELF=='abc'"];
NSArray *array2 = [array filteredArrayUsingPredicate:pre];
```



##### 1.比较运算符(>,<,==,>=,<=,!=)

可用于数值及字符串 例：@"number > 100"

```objectivec
NSArray *array = [NSArray arrayWithObjects:@1,@2,@3,@4,@5,@2,@6, nil];
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF >4"];
NSArray *fliterArray = [array filteredArrayUsingPredicate:predicate];
[fliterArray enumerateObjectsWithOptions:NSEnumerationConcurrent usingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
    NSLog(@"fliterArray = %@",obj);
}];
```

##### 2、运算符

##### 比较和逻辑运算符

| 符号      | 意义    |
| ------- | ----- |
| ==      | 等于    |
| >       | 大于    |
| >= (=>) | 大于或等于 |
| <       | 小于    |
| <= (=<) | 小于或等于 |
| != (<>) | 不等于   |

括号和逻辑运算AND、OR、NOT或者C样式的等效表达式&&、||、!

```
//输出完整的信息，在整个数组里寻找匹配的结果
predicate = [NSPredicate predicateWithFormat:@"age > 100"];
NSArray *results = [self.cars filteredArrayUsingPredicate: predicate];
NSLog (@"%@", results);

//谓词字符窜还支持C语言中一些常用的运算符
predicate = [NSPredicate predicateWithFormat:@"(age > 50) AND (age < 100)"];
results = [self.cars filteredArrayUsingPredicate: predicate];
NSLog (@"C语言中一些常用的运算符~~~~~~~%@", results);


//比较字符串的大小
predicate = [NSPredicate predicateWithFormat:@"name < 'Newton'"];
```



###### 2.1.范围运算符(IN、BETWEEN)

@"number BETWEEN {1,5}" @"address IN {'shanghai','beijing'}"

```objectivec
NSArray *array = [NSArray arrayWithObjects:@1,@2,@3,@4,@5,@2,@6, nil];
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF in {2,5}"]; //找到 in 的意思是array中{2,5}的元素
//NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF BETWEEN {2,5}"];
NSArray *fliterArray = [array filteredArrayUsingPredicate:predicate];
[fliterArray enumerateObjectsWithOptions:NSEnumerationConcurrent usingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
    NSLog(@"fliterArray = %@",obj);
}];
```

##### 3.字符串本身(SELF)

> 类似于SQL语句
> NOT 不是
> SELF 代表字符串本身
> IN 范围运算符
> 那么NOT (SELF IN %@) 意思就是：不是这里所指定的字符串的值

```objectivec
NSArray *placeArray = [NSArray arrayWithObjects:@"Shanghai",@"Hangzhou",@"Beijing",@"Macao",@"Taishan", nil];
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF == 'Beijing'"];
NSArray *tempArray = [placeArray filteredArrayUsingPredicate:predicate];
[tempArray enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
    NSLog(@"obj == %@",obj);
}];

NSPredicate * filterPredicate = [NSPredicate predicateWithFormat:@”NOT (SELF IN %@)”,filteredArray];
//    //过滤数组
NSArray * reslutFilteredArray = [dataArray filteredArrayUsingPredicate:filterPredicate];
NSLog(@”Reslut Filtered Array = %@”,reslutFilteredArray);
```

##### 4字符串相关（BEGINSWITH、ENDSWITH、CONTAINS）

> @"name CONTAIN[cd] 'ang'"  	 //包含某个字符串
> @"name BEGINSWITH[c] 'sh'"     //以某个字符串开头
> @"name ENDSWITH[d] 'ang'"      //以某个字符串结束

`注:[c]不区分大小写[d]不区分发音符号即没有重音符号[cd]既不区分大小写，也不区分发音符号。`

```
NSArray *placeArray = [NSArray arrayWithObjects:@"Shanghai",@"Hangzhou",@"Beijing",@"Macao",@"Taishan", nil];
//    NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF CONTAINS [cd] 'an' "];
    NSPredicate *predicate1 = [NSPredicate predicateWithFormat:@"SELF Beginswith [cd] 'sh' "];
 NSArray *tempArray = [placeArray filteredArrayUsingPredicate:predicate1];
    [tempArray enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        NSLog(@"obj == %@",obj);
    }];
```

##### 5通配符：LIKE

> @"name LIKE[cd] 'er'" //代表通配符,Like也接受[cd]. @"name LIKE[cd] '???er'"
>
> "*"：表示任意多个字符匹配
>
> "?"：表示一个字符匹配

```objective c
NSArray *placeArray = [NSArray arrayWithObjects:@"Shanghai",@"Hangzhou",@"Beijing",@"Macao",@"Taishan", nil];
    NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF  like '*ai*' "];

NSArray *tempArray = [placeArray filteredArrayUsingPredicate:predicate];
[tempArray enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
    NSLog(@"obj == %@",obj);
}];
```

##### 6正则表达式

MATCHES例：

NSString *regex = @"^A.+e$"; //以A开头，e结尾

 @"name MATCHES %@",regex (还是用于其他的正则表达式)

```
NSString *regex = @"^A.+e$";   //以A开头，e结尾  @"name MATCHES %@",regex
  NSPredicate *presdicate =[NSPredicate predicateWithFormat:@"SELF MATCHES %@", regex];
  NSString *content = @"Alkdjflse";
 BOOL result = [presdicate evaluateWithObject:content];
  NSLog(@"%d",result);

 NSPredicate *exists = [NSPredicate predicateWithFormat:
                         @"%K MATCHES[c] %@", key, value];
```

其它

```
// 判断首个字符是否为字母
- (BOOL)isStartWithWord {
  NSString *regex = @"[A-Za-z]+";
  NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", regex];
  return [predicate evaluateWithObject:aString];
}

//用户名是否为字母和数字组成
- (BOOL)isUserName
{
  NSString *regex = @"(^[A-Za-z0-9]{3,20}$)";
  NSPredicate *pred = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", regex];
  return [pred evaluateWithObject:self];
}

//密码是否合法
NSString *regex = @"(^[A-Za-z0-9]{6,20}$)";
//邮箱是否合法
NSString *regex = @"[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,4}";
//url是否合法
NSString *regex = @"http(s)?:\\/\\/([\\w-]+\\.)+[\\w-]+(\\/[\\w- .\\/?%&=]*)?";
```



多规则

```
- (BOOL)isTelephone
{
    NSString * MOBILE = @"^1(3[0-9]|5[0-35-9]|8[025-9])\\d{8}$";
    NSString * CM = @"^1(34[0-8]|(3[5-9]|5[017-9]|8[278])\\d)\\d{7}$";
    NSString * CU = @"^1(3[0-2]|5[256]|8[56])\\d{8}$";
    NSString * CT = @"^1((33|53|8[09])[0-9]|349)\\d{7}$";
    NSString * PHS = @"^0(10|2[0-5789]|\\d{3})\\d{7,8}$";
    NSPredicate *regextestmobile = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", MOBILE];
    NSPredicate *regextestcm = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", CM];
    NSPredicate *regextestcu = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", CU];
    NSPredicate *regextestct = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", CT];
    NSPredicate *regextestphs = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", PHS];

    return  [regextestmobile evaluateWithObject:self]   ||
            [regextestphs evaluateWithObject:self]      ||
            [regextestct evaluateWithObject:self]       ||
            [regextestcu evaluateWithObject:self]       ||
            [regextestcm evaluateWithObject:self];
  }
```



查询字字典，模型属性

```objectivec
 //1 字符串中查出保函某个字节
    NSArray  *array =@[@"123", @"234" , @"345"];
    NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF contains [cd] %@", "2"];
    NSArray *filterdArray1 = [array filteredArrayUsingPredicate:predicate];
    NSLog(@"%@", filterdArray1);


    //2.查找封装model对象的数组，根据model的一个属性
    NSPredicate *predicate2 = [NSPredicate predicateWithFormat:@"fileName == %@", "Ansel"];
    NSArray *filteredArray2 = [array filteredArrayUsingPredicate:predicate2];
    NSLog(@"filteredArray2：%@", filteredArray2);

    //3.查询数组中字典莫一个Key的值
    NSArray *array3 = @[ @{ @"lastName" : @"Turner" },
                         @{ @"firstName" : @"Ben", @"lastName" : @"Ballard",
                            @"birthday": @"1972-03-24 10:45:32 +0600"
                            }
                    ];
    NSPredicate *predicate3 =
    [NSPredicate predicateWithFormat:@"firstName like %@", @"firstName"];
    NSArray *filteredArray3 = [array3 filteredArrayUsingPredicate:predicate3];
```



谓词去重

```objective c
NSMutableSet *seenDates = [NSMutableSet set];
NSPredicate *dupDatesPred = [NSPredicate predicateWithBlock: ^BOOL(id obj, NSDictionary *bind) {
    Event *e = (Event*)obj;
    BOOL seen = [seenDates containsObject:e.date];
    if (!seen) {
        [seenDates addObject:e.date];
    }
    return !seen;
}];
NSArray *events = ... // This is your array which needs to be filtered
NSArray *filtered = [events filteredArrayUsingPredicate:dupDatesPred];
```



参考文档

http://nshipster.cn/nspredicate/





https://stackoverflow.com/questions/805547/how-to-sort-an-nsmutablearray-with-custom-objects-in-it?rq=1

https://stackoverflow.com/questions/111866/best-way-to-remove-from-nsmutablearray-while-iterating?rq=1

https://stackoverflow.com/questions/19373936/how-do-i-get-unique-values-from-an-array、

https://stackoverflow.com/questions/1025674/the-best-way-to-remove-duplicate-values-from-nsmutablearray-in-objectivec

https://stackoverflow.com/questions/4007427/removing-duplicates-from-array-in-objectivec

https://stackoverflow.com/questions/19865936/finding-a-duplicate-numbers-in-an-array-and-then-counting-the-number-of-duplicat

https://stackoverflow.com/questions/5978574/removing-duplicates-from-nsmutablearray

https://stackoverflow.com/questions/20909709/removing-duplicate-profiles-loaded-from-an-xml-file-using-nspredicate

https://stackoverflow.com/questions/43798167/to-get-duplicate-as-well-as-original-items-from-an-array-in-ios