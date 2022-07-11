---
title: iOS中Class相关方法区别
date: 2018-03-23 15:48:55
tags:
categorys: iOS
---

##### 一、`isKindOfClass`、`isMemberOfClass`和`isSubclassOfClass`区别

```objectivec
1.首先申明 四个类 A->B->C->D ，继承关系 A继承B,B继承C，C继承D。即D是最顶层的父类，A是最基层的子类。
A *a = [[A alloc]init];
2.isKindOfClass(对象方法)
[a  isKindOfClass:[A class]]  //return YES
[a  isKindOfClass:[B class]]  //return YES
[a  isKindOfClass:[C class]]  //return YES
[a  isKindOfClass:[D class]]  //return YES

3.isMemberOfClass(对象方法)
[a  isMemberOfClass:[A class]]  //return YES
[a  isMemberOfClass:[B class]]  //return NO
[a  isMemberOfClass:[C class]]  //return NO
[a  isMemberOfClass:[D class]]  //return NO

4.isSubclassOfClass(类方法)
[A  isSubclassOfClass:[A class]]  //return YES
[A  isSubclassOfClass:[B class]]  //return YES
[A  isSubclassOfClass:[C class]]  //return YES
[A  isSubclassOfClass:[D class]]  //return YES
```

总结:isKindOfClass:确定一个对象是否是一个类的成员,或者是派生自该类的成员.(判断是否是这个类或者这个类的子类的实例)

 	isMemberOfClass:确定一个对象是否是当前类的成员.(判断是否是这个类的实例)

##### 二、`object_getClass(obj)`与`[obj class]`区别

```objectivec
void testClassdifference()
{
    
    //obj为实例变量
    NSObject *obj = [NSObject new];
    Class cls = object_getClass(obj);
    Class cls2 = [obj class];
    NSLog(@"实例对象：%p-%p" , cls,cls2); //实例对象：0x107173ea8-0x107173ea8
    //结论：当obj为实例变量时，object_getClass(obj)与[obj class]输出结果一直，均获得isa指针，即指向类对象的指针。
    
    //classObj为类对象
    Class classObj = [obj class];
    Class cls3 = object_getClass(classObj);
    Class cls4 = [classObj class];
    NSLog(@"类对象：%p-%p",cls3,cls4);//类对象：0x107173e58-0x107173ea8
    //结论：当obj为类对象时，object_getClass(obj)返回类对象中的isa指针，即指向元类对象的指针；[obj class]返回的则是其本身。
    
    
    //metaClassObj为元类对象
    Class metaClassObj = object_getClass(classObj);
    Class cls5 = object_getClass(metaClassObj);
    Class cls6 = [metaClassObj class];
    NSLog(@"元类对象：%p-%p",cls5,cls6);//元类对象：0x107173e58-0x107173e58

    //结论：当obj为Metaclass（元类）对象时，object_getClass(obj)返回元类对象中的isa指针，因为元类对象的isa指针指向根类，所有返回的是根类对象的地址指针；[obj class]返回的则是其本身。
    
    //rootClassObj为元类对象
    Class rootClassObj = object_getClass(metaClassObj);
    Class cls7 = object_getClass(rootClassObj);
    Class cls8 = [rootClassObj class];
    NSLog(@"Root元类对象：%p-%p",cls7,cls8);//Root元类对象：0x107173e58-0x107173e58
    //结论：当obj为Rootclass（元类）对象时，object_getClass(obj)返回根类对象中的isa指针，因为跟类对象的isa指针指向Rootclass‘s metaclass(根元类)，即返回的是根元类的地址指针；[obj class]返回的则是其本身。因为根类的isa指针其实是指向本身的，所有根元类其实就是根类，所有输出的结果是一样的。
}

//打印信息：
iOSInterview[8880:1076288] 实例对象：0x107173ea8-0x107173ea8
iOSInterview[8880:1076288] 类对象：0x107173e58-0x107173ea8
iOSInterview[8880:1076288] 元类对象：0x107173e58-0x107173e58
iOSInterview[8880:1076288] Root元类对象：0x107173e58-0x107173e58
```

  总：`object_getClass`(obj)返回的是obj中的isa指针；
  `[obj class]`则分两种情况：

- 一是当obj为实例对象时，`[obj class]中class`是实例方法：- (Class)class，返回的obj对象中的isa指针,也即其本身；

- 二是当obj为类对象（包括元类和根类以及根元类）时，调用的是类方法：+ (Class)class，返回的结果为其本身

  ​