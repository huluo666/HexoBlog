---
title: iOS基础知识
tags:
categories:
date: 2017-03-30 15:17:51
grammar_cjkRuby: true
---

NSString为何要用copy，而不是strong？

NSString、NSArray、NSDictionary等等经常使用copy关键字，是因为他们有对应的`可变类型`：NSMutableString、NSMutableArray、NSMutableDictionary，为确保对象中的属性值不会无意间变动，应该在设置新属性值时拷贝一份，保护其封装性

```objectivec
@property (nonatomic,strong) NSString *str_strong;
@property (nonatomic,copy)   NSString *str_copy;

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    self.str_strong=@"iOS";
    self.str_copy=@"iOS";
    NSMutableString *mstr=[[NSMutableString alloc]initWithString:@"iOS"];
    self.str_strong=mstr;
    self.str_copy=mstr;
    //修改NSMutableString字符串
    [mstr appendString:@"开发"];
    NSLog(@"str_strong=%@",self.str_strong);
    NSLog(@"str_copy=%@",self.str_copy);
}

//NSLog信息
11:34:43.062460+0800 iOSInterview[3915:356342] str_strong=iOS开发
11:34:43.063298+0800 iOSInterview[3915:356342] str_copy=iOS
```



算法

二分查找(也叫折半查找)，是至今应用比较多的一种搜索算法。速度快，比较次数少。

```objectivec
/** 代码执行时间*/
CGFloat TIME_BLOCKWithkey (NSString *key,void (^block)(void)) {
    NSTimeInterval startTime = CACurrentMediaTime();
    block ();
    CFTimeInterval elapsedTime = CACurrentMediaTime() - startTime;
    NSLog(@"%@ executionTime = %f ms",key,elapsedTime *1000.0);
    return (CGFloat)elapsedTime / NSEC_PER_SEC;
}


- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    NSMutableArray *sortedArray=[NSMutableArray array];
    for (int i=0; i<1000000; i++) {
        [sortedArray addObject:@(i)];
    }

    id searchObject =@88888;

    TIME_BLOCKWithkey(@"普通查找", ^{
        NSUInteger findIndex=[sortedArray indexOfObject:searchObject];
        NSLog(@"findIndex=%zd",findIndex);
    });


    TIME_BLOCKWithkey(@"二分查找", ^{
        NSRange searchRange = NSMakeRange(0, [sortedArray count]);
        NSUInteger findIndex = [sortedArray indexOfObject:searchObject
                                            inSortedRange:searchRange
                                                  options:NSBinarySearchingFirstEqual
                                          usingComparator:^(id obj1, id obj2)
                                {
                                    return [obj1 compare:obj2];
                                }];
        NSLog(@"findIndex=%zd",findIndex);
    });
}

//NSLog信息
13:59:23.806081+0800 iOSInterview[5087:608690] 普通查找 executionTime = 8.262457 ms
13:59:23.806208+0800 iOSInterview[5087:608690] findIndex=88888
13:59:23.806340+0800 iOSInterview[5087:608690] 二分查找 executionTime = 0.136384 ms
```

https://www.jianshu.com/p/8a54c26a9349

[C语言二分查找（折半查找）算法及代码](http://c.biancheng.net/cpp/html/2744.html)



一，在程序里，交换2个数，我使用了异或来处理。这个可以根据个人喜好。为了避免产生临时变量，可以使用如下几种方式来交换2个数：

```
// 1.中间变量
void swap(int a, int b) {
	int temp;
 	temp= a;
	  a = b;
	  b = temp;
}

// 2.加法
void swap(int a, int b) {
	a = a + b;
	b = a - b;
	a = a - b;

	or
	a = a+b-(b=a);
}

// 3.异或（相同为0，不同为1. 可以理解为不进位加法）
void swap(int a, int b) {
	a = a ^ b;
	b = a ^ b;
	a = a ^ b;
	或
    a^=b;
    b^=a;
    a^=b

    或
     a ^= b ^= a ^= b;
}

std libs
std::swap(a,b);
```



1. **设计模式是什么？ 你知道哪些设计模式，并简要叙述？**

   ```
   设计模式是一种编码经验，就是用比较成熟的逻辑去处理某一种类型的事情。
   1). MVC模式：Model View Control，把模型 视图 控制器 层进行解耦合编写。
   2). MVVM模式：Model View ViewModel 把模型 视图 业务逻辑 层进行解耦和编写。
   3). 单例模式：通过static关键词，声明全局变量。在整个进程运行期间只会被赋值一次。
   4). 观察者模式：KVO是典型的通知模式，观察某个属性的状态，状态发生变化时通知观察者。
   5). 委托模式：代理+协议的组合。实现1对1的反向传值操作。
   6). 工厂模式：通过一个类方法，批量的根据已有模板生产对象。
   ```

**2、MVC 和 MVVM 的区别**

```
1). MVVM是对胖模型进行的拆分，其本质是给控制器减负，将一些弱业务逻辑放到VM中去处理。
2). MVC是一切设计的基础，所有新的设计模式都是基于MVC进行的改进。
```



一、非集合类对象的copy与mutableCopy

```

  在非集合类对象中，对不可变对象进行copy操作，是指针复制，mutableCopy操作是内容复制；
  对可变对象进行copy和mutableCopy都是内容复制。用代码简单表示如下：
	NSString *str = @"hello word!";
	NSString *strCopy = [str copy] // 指针复制，strCopy与str的地址一样
	NSMutableString *strMCopy = [str mutableCopy] // 内容复制，strMCopy与str的地址不一样

	NSMutableString *mutableStr = [NSMutableString stringWithString: @"hello word!"];
	NSString *strCopy = [mutableStr copy] // 内容复制
	NSMutableString *strMCopy = [mutableStr mutableCopy] // 内容复制
```

```
	只有对不可变对象进行copy操作是指针复制（浅复制），其它情况都是内容复制（深复制）！
```



**什么是 KVO 和 KVC？**

```
1). KVC(Key-Value-Coding)：键值编码 是一种通过字符串间接访问对象的方式（即给属性赋值）
   	举例说明：
   	stu.name = @"张三" // 点语法给属性赋值
   	[stu setValue:@"张三" forKey:@"name"]; // 通过字符串使用KVC方式给属性赋值
   	stu1.nameLabel.text = @"张三";
   	[stu1 setValue:@"张三" forKey:@"nameLabel.text"]; // 跨层赋值
2). KVO(key-Value-Observing)：键值观察机制 他提供了观察某一属性变化的方法，极大的简化了代码。
     KVO只能被KVC触发，包括使用setValue:forKey:方法和点语法。
   // 通过下方方法为属性添加KVO观察
   - (void)addObserver:(NSObject *)observer
                     forKeyPath:(NSString *)keyPath
                     options:(NSKeyValueObservingOptions)options
                     context:(nullable void *)context;
   // 当被观察的属性发送变化时，会自动触发下方方法
   - (void)observeValueForKeyPath:(NSString *)keyPath
                              ofObject:(id)object
                                  change:(NSDictionary *)change
                                 context:(void *)context{}

KVC 和 KVO 的 keyPath 可以是属性、实例变量、成员变量。
```



**ViewController生命周期**



```
按照执行顺序排列：
1. initWithCoder：通过nib文件初始化时触发。
2. awakeFromNib：nib文件被加载的时候，会发生一个awakeFromNib的消息到nib文件中的每个对象。
3. loadView：开始加载视图控制器自带的view。
4. viewDidLoad：视图控制器的view被加载完成。
5. viewWillAppear：视图控制器的view将要显示在window上。
6. updateViewConstraints：视图控制器的view开始更新AutoLayout约束。
7. viewWillLayoutSubviews：视图控制器的view将要更新内容视图的位置。
8. viewDidLayoutSubviews：视图控制器的view已经更新视图的位置。
9. viewDidAppear：视图控制器的view已经展示到window上。
10. viewWillDisappear：视图控制器的view将要从window上消失。
11. viewDidDisappear：视图控制器的view已经从window上消失。
```

http://www.cnblogs.com/bossren/p/6401067.html



**iOS多线程技术有哪几种方式？**

```
答：pthread、NSThread、GCD、NSOperation
```