---
title: iOS 字面量语法
date: 2016-04-20 15:52:33
tags: iOS
grammar_cjkRuby: true
---



```objectivec
NSString *str = [[NSString alloc] initWithFormat:@"%@",@"Hello World"];

//字面量语法
NSString *str = @"Hello World";
```


```objectivec
NSNumber someNumber = [NSNumber numberWithInt:1];

//字面量语法
NSNumber intNumber = @1;
NSNumber floatNumber = @2.5f;
NSNumber doubleNumber = @3.14159;
NSNumber boolNumber = @YES;
NSNumber charNumber = @'a';
```


```objectivec
NSArray animals =[NSArray arrayWithObjects:@"cat", @"dog", @"mouse", @"badger", nil];
//字面量语法
NSArray animals = @[@"cat", @"dog", @"mouse", @"badger"];


NSString dog = [animals objectAtIndex:1];
//---->通过字面值取数据
NSString dog = animals[1];


//可变的话可以直接设置新值
[mutableArray replaceObjectAtIndex:1 withObject:@"dog"];
//---->字面值设置值
mutableArray[1] = @"dog";


NSString str1;
NSString str2 = @"1";
NSString *str3 = @"1";


//插入空数据会抛出异常
NSArray arr = @[str1,str2,str3];
//reason: '* -[__NSPlaceholderArray initWithObjects:count:]: attempt to insert nil object from objects[0]'

//这种方式不会抛异常，但是数组会收到第一个nil就终止了。所以采用字面值形式会得到错误提示。
NSArray arr = [NSArray arrayWithObjects:str1,str2,str3, nil];
NSLog(@"arr.count=%d",arr.count); //0
NSLog(@"arr.first=%@",arr.firstObject);  //null
```


```objectivec
NSDictionary personData =[NSDictionary dictionaryWithObjectsAndKeys:@"Kevin", @"firstName",@"Jin", @"lastName",[NSNumber numberWithInt:25], @"age", nil];
//---->字面值如下
NSDictionary personData = @{@"firstName" : @"Kevin",@"lastName" : @"Jin", @"age" : @25};


NSString str1;
NSString str2 = @"1";
NSString str3 = @"1";
//和数组一样，value不能有nil ，否则就会抛出异常。
NSDictionary dic =@{@"str1":str1,@"str2":str2,@"str3":str3};
//reason: ' -[__NSPlaceholderDictionary initWithObjects:forKeys:count:]: attempt to insert nil object from objects[0]'

//不会提示你错误
NSDictionary dic = [NSDictionary dictionaryWithObjectsAndKeys:str1,@"str2",str2,@"str2",str3,@"str3", nil];
NSLog(@"dic.count=%d",dic.count); //0

//取值
NSString lastName = [personData objectForKey:@"lastName"];
//---->字面值取值
NSString lastName = personData[@"lastName"];

//可变的话可以这样设置新值
[mutableDictionary setObject:@"Galloway" forKey:@"lastName"];
//---->字面值设置值
mutableDictionary[@"lastName"] = @"Galloway";
```