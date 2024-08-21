---
title: 利用runtime进行自动归档解档
date: 2016-05-24 19:32:37
tags: iOS
grammar_cjkRuby: true
---

> 基本流程--利用runtime获取属性(成员变量)—遍历元素利用KVC逐个赋值取值。

### 1、归档解档runtime具体实现

1.重写`-(void)encodeWithCoder:(NSCoder *)aCoder`和`-(id)initWithCoder:(NSCoder *)aDecoder`方法。

2.自定义方法，然后在encodeWithCoder与initWithCoder中调用

虽然第一个方法省事，但不建议使用重写覆盖系统方法。

```objectivec

#pragma -解档
- (void)decode: (NSCoder *)decoder{
    Class cla = self.class;
    while (cla && cla != [NSObject class]) { //不归档NSObject的属性
        unsigned int outCount = 0;
        objc_property_t *pList = class_copyPropertyList(cla, &outCount);

        for (int i = 0; i < outCount; ++i) {
            NSString *name = [NSString stringWithUTF8String:property_getName(pList[i])];
            //添加不解档的属性
            if ([self respondsToSelector:@selector(ignoredNames)]) {
                if ([[self ignoredNames] containsObject:name]) continue;
            }
            id value = [decoder decodeObjectForKey:name];//进行解档取值
            if (value) {
                [self setValue:value forKey:name];   //利用KVC对属性赋值
            }
        }
        free(pList);
        //获得父类
        cla = [cla superclass];
    }
}
```

```objectivec
#pragma mark-归档
- (void)encode: (NSCoder *)encoder{
    Class cla = self.class;
    while (cla && cla != [NSObject class]) { //不归档NSObject的属性
        unsigned int outCount = 0;
        objc_property_t *pList = class_copyPropertyList(cla, &outCount);
        for (int i = 0; i < outCount; ++i) {
            NSString *name = [NSString stringWithUTF8String:property_getName(pList[i])];
            //添加不归档的属性
            if ([self respondsToSelector:@selector(ignoredNames)]) {
                if ([[self ignoredNames] containsObject:name]) continue;
            }
            id value = [self valueForKeyPath:name];
            if (value) {
                [encoder encodeObject:value forKey:name];
            }
        }
        free(pList);
        //获得父类
        cla = [cla superclass];
    }
}
```



### 2.为减少模型中重复代码使用，可以直接宏定义

```objectivec
#define AutoCodingImplementation \
-(id)initWithCoder:(NSCoder *)aDecoder{\
if (self = [super init]) {\
[self decode:aDecoder];\
}\
return self;\
}\
\
- (void)encodeWithCoder:(NSCoder *)aCoder{\
[self encode:aCoder];\
}
```



#### 3.使用方法

比如User模型

第1步:遵守协议 `User.h`中
 `@interface User : NSObject<NSCoding>`

```
@interface User : NSObject<NSCoding>
@property (nonatomic,copy) NSString *name;
@property (nonatomic,assign) double height;
@end
```

第2步: 添加忽略属性 ` User.m`

第3步: 添加自动归档解档宏`User.m`

```objectivec
@implementation User
// 设置需要忽略的属性
- (NSArray *)ignoredNames {
    return @[@"height"];
}
AutoCodingImplementation
@end
```



第4步：归档,解档

```objectivec
/** 归档，解档*/
-(void)autoCodingWithModel:(User *)user
{
    //1.归档
    NSData *modelData = [NSKeyedArchiver archivedDataWithRootObject:user];
    //2.解档
    User *model = [NSKeyedUnarchiver unarchiveObjectWithData:modelData];
}
```