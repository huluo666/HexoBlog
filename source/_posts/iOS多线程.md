---
title: iOS多线程
date: 2018-03-26 12:10:53
tags:
categorys:
---

在 iOS 中其实目前有 `4` 套多线程方案

- NSThread
- GCD
- NSOperation & NSOperationQueue
- Pthreads(基于**c语言**的框架,不常用)




#### 基本概念

| 名称   | 概念                                       |
| ---- | ---------------------------------------- |
| 进程   | 一个具有一定独立功能的程序关于某个数据集合的一次运行活动。可以理解成一个运行中的应用程序 |
| 线程   | 程序执行流的最小单元，线程是进程中的一个实体。                  |
| 同步   | 只能在当前线程按先后顺序依次执行，不开启新线程。                 |
| 异步   | 可以在当前线程开启多个新线程执行，可不按顺序执行。  队列： 装载线程任务的队形结构。  并发： 线程执行可以同时一起进行执行。  串行： 线程执行只能依次逐一先后有序的执行。 |
| 队列   | 装载线程任务的队形结构。  并发： 线程执行可以同时一起进行执行。  串行： 线程执行只能依次逐一先后有序的执行。 |
| 并发   | 线程执行可以同时一起进行执行。                          |
| 串行   | 线程执行只能依次逐一先后有序的执行。                       |

> 备注：一个进程可有多个线程->一个进程可有多个队列->队列可分并发队列和串行队列。

#### 一、NSThread

这套方案是经过苹果封装后的，并且完全面向对象的。

```objectivec
// 方法一：创建线程，需要自己开启线程
NSThread *thread = [[NSThread alloc]initWithTarget:self selector:@selector(run) object:nil];
thread.threadPriority = 1;// 设置线程的优先级(0.0 - 1.0，1.0最高级)
// 开启线程
[thread start];

// 方法二：静态实例化-创建线程后自动启动线程
[NSThread detachNewThreadSelector:@selector(run) toTarget:self withObject:nil];

// 方法三：隐式实例化-隐式创建并启动线程
[self performSelectorInBackground:@selector(run) withObject:nil];
```



#### 二、GCD(英文全称:Grand Central Dispatch)

- GCD是苹果开发的一个多核编程的解决方法
- GCD是苹果推荐而且最简洁的;
- 纯C语言，提供了非常多强大的函数；
- GCD会自动利用更多的CPU内核（比如双核、四核);
- GCD会自动管理线程的生命周期（创建线程、调度任务、销毁线程）;

GCD是基于C的API，因此比较底层。

GCD所管理的调度队列（dispatch queue）主要有三类，1、**串行队列**（private dispatch queue）、2、**并发队列** （global dispatch queue，又称全局调度队列）和**主队列**（main dispatch queue）。

**队列与任务**

GCD有四个概念：串行队列、并行队列、同步、异步四者。

`同步（sync）` 和 `异步（async）` 的主要区别在于会不会阻塞当前线程，直到 `Block` 中的任务执行完毕！

**同步(sync)** ：它会阻塞当前线程并等待 `Block` 中的任务执行完毕，然后当前线程才会继续往下运行。

**异步(async)**：当前线程会直接往下执行，它不会阻塞当前线程。

**队列**：用于存放任务。一共有两种队列， **串行队列** 和 **并行队列**。

队列**：用于存放任务。一共有两种队列， **串行队列** 和 **并行队列**。



| 名称   | 简介                                       |
| ---- | ---------------------------------------- |
| 同步   | 完成需要做的任务后才会返回，进行下一任务(“任务”，在 GCD 里指的是 Block；在 performSelector 方法中，对应 selector 方法。同步方法，功能类似 `dispatch_group_wait` ，而 group 指的是所有线程，包括主线程。) |
| 异步   | 不会等待任务完成才返回，会立即返回。(异步是多线程的代名词，因为必定会开启新的线程，线程的申请是由异步负责，起到开分支的作用。) |
| 串行队列 | 任务依次执行(同一时间队列中只有一个任务在执行，每个任务只有在前一个任务执行完成后才能开始执行。) |
| 并行队列 | 任务并发执行(你唯一能保证的是，这些任务会按照被添加的顺序开始执行。但是任务可以以任何顺序完成) |
| 全局队列 | 隶属于并行队列,不要与 barrier 栅栏方法搭配使用， barrier 只有与自定义的并行队列一起使用，才能让 barrier 达到我们所期望的栅栏功能。与 串行队列或者 global 队列 一起使用，barrier 的表现会和 dispatch_sync 方法一样。 |
| 主队列  | 隶属于串行队列,不能与 sync 同步方法搭配使用，会造成死循环（ 主队列是GCD自带的一种特殊的串行队列，放在主队列中的任务，都会放到主线程中执行） |

**自定义队列**

```
Serial Dispatch Queue:串行调度队列
Concurrent Dispatch Queue:并发调度队列
Serial:串行 Concurrent:同时   并发:Concurrency  并行:Parallelism
```

并发和并行都是指多个任务可以同时执行，都属于多线程编程概念，因此二者必然十分相近，容易混淆。二者区别只有一点，即是否多任务执行于严格的同一时刻。并发不是，并行是。

| 说明                               | Dispatch Queue分类          |
| -------------------------------- | ------------------------- |
| 串行的队列，每次只能执行一个任务，并且必须等待前一个执行任务完成 | Serial Dispatch Queue     |
| 一次可以并发执行多个任务，不必等待执行中的任务完成        | Concurrent Dispatch Queue |

|      | 同步执行                 | 异步执行                                     |
| ---- | -------------------- | ---------------------------------------- |
| 串行队列 | 当前线程，一个一个执行( 不会新建线程) | 其他线程，一个一个执行, 每次使用 createDispatch 方法就会新建一条线程, 多条线程间会并行执行 |
| 并发队列 | 当前线程，一个一个执行( 不会新建线程) | 开很多线程，一起执行( iOS7-SDK 时代一般是5、6条， iOS8-SDK 以后可以50、60条) |

[并发与并行的区别？](https://www.zhihu.com/question/33515481)



GCD使用步骤：

1. 定制任务
   确定想做的事情，在 GCD 里指的是 Block
2. 将任务添加到队列中
   GCD会自动将队列中的任务取出，放到对应的线程中执行
   任务的取出遵循队列的FIFO原则：先进先出，后进后出



### 创建队列

iOS 中, 队列主要分为:

- 全局队列(dispatch_get_main_queue)

- 主队列(dispatch_get_global_queue)

- 自定义队列(dispatch_queue_create)

  ①串行队列(dispatch_queue_create-DISPATCH_QUEUE_SERIAL)

  ②并发队列(dispatch_queue_create-DISPATCH_QUEUE_CONCURRENT)

##### 1、主队列(串行队列)

它用于刷新 UI，任何需要刷新 UI 的工作都要在主队列执行，所以一般耗时的任务都要放到别的线程执行。

```
dispatch_queue_t queue = dispatch_get_main_queue;
```

##### 2、全局并行队列

只要是并行任务一般都加入到这个队列。

```
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
```

##### 3、自定义队列

```
//串行队列
dispatch_queue_t queue = dispatch_queue_create("com.gcd.serialQueue", NULL);
dispatch_queue_t queue = dispatch_queue_create("com.gcd.serialQueue", DISPATCH_QUEUE_SERIAL);

//并发(行)队列
dispatch_queue_t queue = dispatch_queue_create("com.gcd.conQueue", DISPATCH_QUEUE_CONCURRENT);
```

### 创建任务

- 分为同步任务和异步任务

```
//串行
dispatch_queue_t queue = dispatch_queue_create("serial_queue", DISPATCH_QUEUE_SERIAL);
//并发
dispatch_queue_t queue = dispatch_queue_create("serial_queue", DISPATCH_QUEUE_CONCURRENT);

//同步
dispatch_sync(queue, ^{
  NSLog(@"%@", [NSThread currentThread]);
});

//异步
dispatch_async(queue, ^{
   NSLog(@"%@", [NSThread currentThread]);
});
```



#### NSOperation

NSOperation 是苹果公司对 GCD 的封装，完全面向对象，`NSOperation` 和 `NSOperationQueue` 分别对应 GCD 的任务和队列 。操作步骤也很好理解：

- ①将要执行的任务封装到一个 NSOperation 对象中。


- ②将此任务添加到一个 NSOperationQueue 对象中。



NSOperation 只是一个抽象类，所以不能封装任务。但它有 2 个子类用于封装任务。分别是：`NSInvocationOperation` 和 `NSBlockOperation` 。创建一个 Operation 后，需要调用 start 方法来启动任务，它会 默认在当前队列同步执行。当然你也可以在中途取消一个任务，只需要调用其 `cancel`方法即可。

```objective c
//1.创建NSInvocationOperation对象
NSInvocationOperation *operation = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(run) object:nil];

//2.开始执行
[operation start];

//1.创建NSBlockOperation对象
NSBlockOperation *operation = [NSBlockOperation blockOperationWithBlock:^{
    NSLog(@"%@", [NSThread currentThread]);
}];

//2.开始任务
[operation start];
```

##### NSOperationQueue

NSOperationQueue是对GCD的objectivec封装，相对于GCD具有更多先进的特性，如可以添加NSOperation依赖，取消NSOperation等。

NSOperationQueue是并发队列，且不遵循先进先出FIFO排序原则。



### 两种队列(NSOperation)

NSOperationQueue 有两种不同类型的队列：主队列和自定义队列。主队列运行在主线程之上，而自定义队列在后台执行。在两种类型中，这些队列所处理的任务都使用 NSOperation 的子类来表述。

```
NSOperationQueue *mainQueue = [NSOperationQueue mainQueue];  //主队列
NSOperationQueue *queue = [[NSOperationQueue alloc] init]; //自定义队列

//添加一个NSOperation
[queue addOperation:operation]

//添加一组NSOperation
[queue addOperations:operations waitUntilFinished:NO

//添加一个block形式的Operation
[queue addOperationWithBlock:^{
    //执行一个Block的操作
}];

[queue setMaxConcurrentOperationCount:1];

//单个NSOperation取消
[operation cancel]

//取消NSOperationQueue中的所有操作
[queue cancelAllOperations]

// 暂停queue
[queue setSuspended:YES];
```

NSOperation 有一个非常实用的功能，那就是添加依赖。比如有 3 个任务：A: 从服务器上下载一张图片，B：给这张图片加个水印，C：把图片返回给服务器。这时就可以用到依赖了:

```
//1.任务一：下载图片
NSBlockOperation *operation1 = [NSBlockOperation blockOperationWithBlock:^{
    NSLog(@"下载图片 - %@", [NSThread currentThread]);
    [NSThread sleepForTimeInterval:1.0];
}];

//2.任务二：打水印
NSBlockOperation *operation2 = [NSBlockOperation blockOperationWithBlock:^{
    NSLog(@"打水印   - %@", [NSThread currentThread]);
    [NSThread sleepForTimeInterval:1.0];
}];

//3.任务三：上传图片
NSBlockOperation *operation3 = [NSBlockOperation blockOperationWithBlock:^{
    NSLog(@"上传图片 - %@", [NSThread currentThread]);
    [NSThread sleepForTimeInterval:1.0];
}];

//4.设置依赖
[operation2 addDependency:operation1];      //任务二依赖任务一
[operation3 addDependency:operation2];      //任务三依赖任务二

//5.创建队列并加入任务
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
[queue addOperations:@[operation3, operation2, operation1] waitUntilFinished:NO];
```





http://jumpingfrog0.github.io/2017/2017-04-20-iOS%E5%A4%9A%E7%BA%BF%E7%A8%8B%E6%80%BB%E7%BB%93/

https://blog.cnbluebox.com/blog/2014/07/01/cocoashen-ru-xue-xi-nsoperationqueuehe-nsoperationyuan-li-he-shi-yong/

[iOS开发系列--并行开发其实很容易](http://www.cnblogs.com/kenshincui/p/3983982.html)