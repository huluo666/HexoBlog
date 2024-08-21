---
title: KVO原理及实现
date: 2018-03-22 16:17:03
tags:
categorys: iOS
---

KVO的实现原理简述：
​	当一个类的属性被观察的时候，系统会通过runtime动态的创建一个该类的派生类**NSKVONotifying_A**，并且会在这个类中重写基类被观察的属性的setter方法，而且系统将这个类的isa指针(`object_getClass(obj)`)指向了派生类，从而实现了给监听的属性赋值时调用的是派生类的setter方法。重写的setter方法会在调用原setter方法前后，通知观察对象值得改变。

Person.h

```objectivec
#import <Foundation/Foundation.h>
#import <UIKit/UIKit.h>

@interface Person : NSObject

@property (nonatomic,copy)    NSString *userName;
@property (nonatomic,assign)  NSInteger age;
@property (nonatomic,assign)  CGFloat height;

//手动触发KVO
-(void)manualTriggerKVO;

@end
```

Person.m

```objectivec
#import "Person.h"
#import <objc/runtime.h>


@implementation Person

/**
 *  如果重写，这两个方法，kvo就失效了。
 */
//- (void)willChangeValueForKey:(NSString *)key{
//    NSLog(@"willChangeValueForKey");
//}

//- (void)didChangeValueForKey:(NSString *)key{
//    NSLog(@"didChangeValueForKey");
//}

-(void)manualTriggerKVO
{
    //“手动触发userName的KVO”，必写。
    [self willChangeValueForKey:@"userName"];

    // “手动触发userName的KVO”，必写。
    [self didChangeValueForKey:@"userName"];
}

//options属性改变change的值，这个是观察者要实现的方法。
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSString *,id> *)change context:(void *)context{
    if ([keyPath isEqualToString:@"age"]) {
        NSLog(@"%@:%@",self.class,change);
    }
}

@end
```



```objectivec
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view from its nib.
    Person* one = [Person new];
    Person* two = [Person new];
    Person* three = [Person new];

    printf("注册观察者之前\n");
    PrintDescription(@"one",one);
    PrintDescription(@"two",two);
    PrintDescription(@"three",three);
    PrintMethodImp(@"one",one, @selector(setUserName:));
    PrintMethodImp(@"two",two, @selector(setUserName:));
    PrintMethodImp(@"three",three, @selector(setUserName:));

    [one addObserver:one forKeyPath:@"age"
             options:NSKeyValueObservingOptionNew
             context:nil];
    [two addObserver:two forKeyPath:@"height"
             options:NSKeyValueObservingOptionNew
             context:nil];

    printf("注册观察者之后\n");
    Class cls=  object_getClass(one);
    NSLog(@"\n\t父类：%@",cls.superclass);

    PrintDescription(@"one",one);
    PrintDescription(@"two",two);
    PrintDescription(@"three",three);
    PrintMethodImp(@"one",one, @selector(setAge:));
    PrintMethodImp(@"two",two, @selector(setAge:));
    PrintMethodImp(@"three",three, @selector(setAge:));

    one.age=18;
    two.age=25;
    three.age=43;
    printf("赋值观察者之后\n");
    PrintDescription(@"one",one);
    PrintDescription(@"two",two);
    PrintDescription(@"three",three);
    PrintMethodImp(@"one",one, @selector(setAge:));
    PrintMethodImp(@"two",two, @selector(setAge:));
    PrintMethodImp(@"three",three, @selector(setAge:));
}


#pragma mark--打印函数
/** 获取类的方法列表*/
static inline NSArray *classMethodList(Class aclass) {
    NSMutableArray* array = [NSMutableArray array];
    unsigned int count = 0;
    Method* methodList = class_copyMethodList(aclass, &count);
    for(int i = 0; i < count; ++i) {
        SEL sel = method_getName(*(methodList+i));
        [array addObject:NSStringFromSelector(sel)];
    }
    free(methodList);
    return array;
}



/** 打印对象的信息*/
static inline NSString *PrintDescription(NSString *name,id obj) {
    NSString* string = [NSString stringWithFormat:@"\t%@:%@\n\t表面的类: %@\n\t真实类型: %@\n\t方法列表: %@\n\t方法列表2: %@\n",
                        name,
                        obj,
                        [obj class],
                        object_getClass(obj),
                        [classMethodList(object_getClass(obj)) componentsJoinedByString:@","],
                        [classMethodList([obj class]) componentsJoinedByString:@","]
                        ];

    printf("%s\n", [string UTF8String]);
    return string;
}


/** 打印方法方法实现指针*/
static void PrintMethodImp(NSString *name,id cls,SEL  sel)
{
    IMP imp1 =class_getMethodImplementation([cls class],sel);
    IMP imp11 =class_getMethodImplementation(object_getClass(cls),sel);
    NSString *selName=NSStringFromSelector(sel);
    printf("%s方法名%s：Class=%p subClass:%p\n",[name  UTF8String],[selName  UTF8String],imp1,imp11);
}
```

打印信息如下：

```shell
注册观察者之前
	one:<Person: 0x60000002a8a0>
	表面的类: Person
	真实类型: Person
	方法列表: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age
	方法列表2: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age

	two:<Person: 0x6000000296c0>
	表面的类: Person
	真实类型: Person
	方法列表: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age
	方法列表2: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age

	three:<Person: 0x60000002fae0>
	表面的类: Person
	真实类型: Person
	方法列表: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age
	方法列表2: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age

one方法名setUserName:：Class=0x10f528710 subClass:0x10f528710
two方法名setUserName:：Class=0x10f528710 subClass:0x10f528710
three方法名setUserName:：Class=0x10f528710 subClass:0x10f528710
注册观察者之后
2018-03-26 11:00:57.025622+0800 iOSInterview[3214:356624]
	父类：Person
	one:<Person: 0x60000002a8a0>
	表面的类: Person
	真实类型: NSKVONotifying_Person
	方法列表: setHeight:,setAge:,setUserName:,class,dealloc,_isKVOA
	方法列表2: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age

	two:<Person: 0x6000000296c0>
	表面的类: Person
	真实类型: NSKVONotifying_Person
	方法列表: setHeight:,setAge:,setUserName:,class,dealloc,_isKVOA
	方法列表2: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age

	three:<Person: 0x60000002fae0>
	表面的类: Person
	真实类型: Person
	方法列表: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age
	方法列表2: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age

one方法名setAge:：Class=0x10f528770 subClass:0x10f886106
two方法名setAge:：Class=0x10f528770 subClass:0x10f886106
three方法名setAge:：Class=0x10f528770 subClass:0x10f528770
2018-03-26 11:00:57.026266+0800 iOSInterview[3214:356624] Person:{
    kind = 1;
    new = 18;
}
赋值观察者之后
	one:<Person: 0x60000002a8a0>
	表面的类: Person
	真实类型: NSKVONotifying_Person
	方法列表: setHeight:,setAge:,setUserName:,class,dealloc,_isKVOA
	方法列表2: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age

	two:<Person: 0x6000000296c0>
	表面的类: Person
	真实类型: NSKVONotifying_Person
	方法列表: setHeight:,setAge:,setUserName:,class,dealloc,_isKVOA
	方法列表2: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age

	three:<Person: 0x60000002fae0>
	表面的类: Person
	真实类型: Person
	方法列表: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age
	方法列表2: setUserName:,userName,.cxx_destruct,printInfo,observeValueForKeyPath:ofObject:change:context:,height,setHeight:,setAge:,age

one方法名setAge:：Class=0x10f528770 subClass:0x10f886106
two方法名setAge:：Class=0x10f528770 subClass:0x10f886106
three方法名setAge:：Class=0x10f528770 subClass:0x10f528770
```

#### 总结：

1、在未添加观察者之前，运行时的类和对象本身的类是一样的。添加KVO后生成NSKVONotifying_Person类，打印NSKVONotifying_Person父类(superclass)为Person，表明**NSKVONotifying_A**为**A**的派生类。

2、打印方法列表，one,two`[obj class]`与`object_getClass(obj)`的方法列表不同，而没有监听属性的three一切正常。可以看到注册KVO后isa指针(`object_getClass(obj`)对象为NSKVONotifying_Person，并重写了了setHeight，setAge等setter方法,从而利用`willChangeValueForKey`与`didChangevlueForKey` 来激活键值通知机制。

Apple 使用了 isa 混写（isa-swizzling）来实现 KVO 。当观察对象A时，KVO机制动态创建一个新的名为：**NSKVONotifying_A** 的新类，该类继承自对象A的本类，且 KVO 为 **NSKVONotifying_A** 重写观察属性的 setter 方法，setter 方法会负责在调用原 setter 方法之前和之后，通知所有观察对象属性值的更改情况。

（备注： isa 混写（isa-swizzling）isa：is a kind of ； swizzling：混合，搅合；）

KVO 的键值观察通知依赖于 NSObject 的两个方法:`willChangeValueForKey`:和 `didChangevlueForKey`:，在存取数值的前后分别调用。

#### 参考文档

https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html