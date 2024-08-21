---
title: 判断是否主线程
date: 2017-09-04 15:13:38
tags: iOS
categories: [iOS]
grammar_cjkRuby: true
---

```objectivec
[[NSThread currentThread] isMainThread] ? NSLog(@"MAIN THREAD") : NSLog(@"NOT MAIN THREAD");

Also,
[[NSThread mainThread] isEqual:[NSThread currentThread]] ? NSLog(@"MAIN THREAD") : NSLog(@"NOT MAIN THREAD");

```



保证主线程运行

```objectivec
void ensureOnMainQueue(void (^block)(void)) {
    if ([[NSOperationQueue currentQueue] isEqual:[NSOperationQueue mainQueue]]) {
        block();
    } else {
        [[NSOperationQueue mainQueue] addOperationWithBlock:^{
            block();
        }];
    }
}
```



```objectivec
   if (strcmp(dispatch_queue_get_label(DISPATCH_CURRENT_QUEUE_LABEL), dispatch_queue_get_label(dispatch_get_main_queue())) == 0) { // do something in main thread
    } else {
        // do something in other thread
    }

```

主线程中也不绝对安全的 UI 操作

http://www.jianshu.com/p/d15f4b37b0f2
https://stackoverflow.com/questions/3546539/check-whether-or-not-the-current-thread-is-the-main-thread
https://stackoverflow.com/questions/17475002/get-current-dispatch-queue