---
title: iOS代码执行时间
date: 2017-12-27 09:53:14
tags:
categorys:
---



在测试代码效率是往往会用到

StackOverflow [How to log a method's execution time exactly in milliseconds?](https://stackoverflow.com/questions/2129794/how-to-log-a-methods-execution-time-exactly-in-milliseconds)

```json
//单位换算
1分(min)=60秒(s)
1秒(s)=1000毫秒(ms)
1毫秒(ms)=1000微秒(μs)
1微秒(μs)=1000纳秒(ns)
```



一、CFAbsoluteTime （纳秒级的精确度 ）

```objectivec
CFAbsoluteTime startTime =CFAbsoluteTimeGetCurrent();
//在这写入要计算时间的代码
...
CFAbsoluteTime executionTime = (CFAbsoluteTimeGetCurrent() - startTime);
NSLog(@"executionTime %f ms", executionTime *1000.0);
//打印出来代码执行时间 单位ms(executionTime 7.062972 ms)
```



block方式

```objective c
TIME_BLOCK(^{
    //执行需要测试的代码
});


CGFloat TIME_BLOCK(void (^block)(void)){
    NSTimeInterval startTime = CACurrentMediaTime();
    block ();
    CFTimeInterval elapsedTime = CACurrentMediaTime() - startTime;
    NSLog(@"executionTime = %f ms",  elapsedTime *1000.0);
    return (CGFloat)elapsedTime / NSEC_PER_SEC;
}


//key 标识符
CGFloat TIME_BLOCKWithkey (NSString *key,void (^block)(void)) {
    NSTimeInterval startTime = CACurrentMediaTime();
    block ();
    CFTimeInterval elapsedTime = CACurrentMediaTime() - startTime;
    NSLog(@"%@ executionTime = %f ms",key,elapsedTime *1000.0);
    return (CGFloat)elapsedTime / NSEC_PER_SEC;
}
```

二、NSDate

```objective c
NSDate *methodStart = [NSDate date];
/* ... Do whatever you need to do ... */
NSDate *methodFinish = [NSDate date];
NSTimeInterval executionTime = [methodFinish timeIntervalSinceDate:methodStart];
NSLog(@"executionTime = %f ms",  executionTime *1000.0);
//打印出来为代码执行时间 单位ms(7.090032 ms)
```

简写宏

```objectivec
宏定义
#define TICK   NSDate *startTime = [NSDate date]
#define TOCK   NSLog(@"Time: %f", -[startTime timeIntervalSinceNow])

TICK
/* ... Do Some Work Here ... */
TOCK
```



三、`CACurrentMediaTime()`返回的精度-微秒级别

 CACurrentMediaTime方法获取到的时间，是手机从开机一直到当前所经过的秒数。

```
NSTimeInterval startTime = CACurrentMediaTime();

// your code goes here
CFTimeInterval elapsedTime = CACurrentMediaTime() - startTime;
NSLog(@"executionTime = %f ms",  elapsedTime *1000.0);
//打印出来为代码执行时间 单位ms(7.299827 ms)
```

- NSDate 属于Foundation
- CFAbsoluteTimeGetCurrent() 属于 CoreFoundatio
- CACurrentMediaTime() 属于 QuartzCore



第二种：（将运行代码放入下面的Block中，返回时间）

```
#import <mach/mach.h>
#import <mach/mach_time.h>  // for mach_absolute_time() and friends

CGFloat TIME_BLOCK (void (^block)(void)) {
    mach_timebase_info_data_t info;
    if (mach_timebase_info(&info) != KERN_SUCCESS) return -1.0;

    uint64_t start = mach_absolute_time ();
    block ();
    uint64_t end = mach_absolute_time ();
    uint64_t elapsed = end - start;

    uint64_t nanos = elapsed * info.numer / info.denom;
    return (CGFloat)nanos / NSEC_PER_SEC;
}

//加入key
CGFloat TIMEKey_BLOCK(NSString *key, void (^block)(void)) {
    mach_timebase_info_data_t info;
    if (mach_timebase_info(&info) != KERN_SUCCESS)
    {
        return -1.0;
    }

    uint64_t start = mach_absolute_time();
    block();
    uint64_t end = mach_absolute_time();
    uint64_t elapsed = end - start;

    uint64_t nanos = elapsed * info.numer / info.denom;
    float cost = (float)nanos / NSEC_PER_SEC;

    NSLog(@"key: %@ (%f ms)\n", key, cost * 1000);
    return cost;
}


uint64_t getTickCount(void)
{
    static mach_timebase_info_data_t sTimebaseInfo;
    uint64_t machTime = mach_absolute_time();

    // Convert to nanoseconds - if this is the first time we've run, get the timebase.
    if (sTimebaseInfo.denom == 0 )
    {
        (void) mach_timebase_info(&sTimebaseInfo);
    }

    // Convert the mach time to milliseconds
    uint64_t millis = ((machTime / 1000000) * sTimebaseInfo.numer) / sTimebaseInfo.denom;
    return millis;
}
```





```
/**
 * 计算脚本时间
 * @param $last 最后一次的运行clock
 * @param $key  标识
 * @return 当前clock
 */
double timeNow(double last, char* key){
    clock_t now = clock();
    printf("time:%fs \t key:%s \n", (last != 0) ? (double)(now - last) / CLOCKS_PER_SEC : 0, key);
    return now;
}

double t1 = t(0, "");
//do something
t(t1, "end");
```

参考：

[iOS关于时间的处理](https://zhuanlan.zhihu.com/p/24377367?refer=mrpeak)