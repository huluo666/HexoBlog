---
title: NSTask 使用
tags: Mac
categories: Mac
date: 2017-08-02 18:08:43
---

创建和初始化一个NSTask对象

一些属性 返回任务信息

```objectivec
@property(copy) NSArray<NSString *> *arguments  //NSString对象数组包含调用时使用的参数。如果参数是nill,产生一个NSInvalidArgumentException异常
@property(copy) NSString *currentDirectoryPath  //任务当前的工作目录
@property(copy) NSDictionary<NSString *,NSString *> *environment //任务启动后的环境变量字典，字典的key是环境变量的名称
@property(copy) NSString *launchPath            //可执行文件的路径
@property(readonly) int processIdentifier       //进程标识
@property(retain) id standardError              //标准错误文件
@property(retain) id standardInput  //任务使用的标准输入文件，除非另有说明。返回的对象是一个NSFileHandle或者一个NSPipe实例,这取决于传递给setStandardInput:方法哪种类型的对象。
@property(retain) id standardOutput //任务使用的标准输出文件，标准输出用来作为任务显示其输出。返回的对象是一个NSFileHandle或者一个NSPipe实例,这取决于传递给setStandardOutput:方法哪种类型的对象。

// actions
- (void)launch;//启动任务
- (void)interrupt; // 发送一个中断信号给接受者和它的所有子任务如果任务因此终止，这是默认的行为，一个NSTaskDidTerminateNotification发送到默认的通知中心。如果消息接收者已经启动或者已经执行完成，这个方法没有效果。如果消息接收者尚未启动，该方法发出了一个NSInvalidArgumentException。
- (void)terminate; // Not always possible. Sends SIGTERM.
- (BOOL)suspend;//暂停任务,如果任务暂停成功 返回YES，否则NO
- (BOOL)resume;//恢复执行之前suspend的任务。

@property (readonly, getter=isRunning) BOOL running;//查询任务状态 返回任务是否仍在运行

waitUntilExit	block,直到任务完成。
该方法首先检查是否有任务在使用 isRunning标识运行。然后阻塞当前run loop（使用NSDefaultRunLoopMode）直到任务完成。
terminationReason  返回被终止的原因
```

1、使用NSString类型的命令字符串，返回运行结果。但是使用这种方法没法记忆上一次操作，没法做到像在终端中执行多次命令那样自如。

例如：先cd到桌面，然后在桌面新建文件夹，在终端中我们是这么实现的：

```shell
$ huluo-Mac:~ luo.h$ cd Desktop
$ huluo-Mac:Desktop luo.h$ mkdir helloWorld
```



使用NSTask调用：

```shell
- (NSString *)cmd:(NSString *)cmd
{
    // 初始化并设置shell路径
    NSTask *task = [[NSTask alloc] init];
    [task setLaunchPath: @"/bin/bash"];
    // -c 用来执行string-commands（命令字符串），也就说不管后面的字符串里是什么都会被当做shellcode来执行
    NSArray *arguments = [NSArray arrayWithObjects: @"-c", cmd, nil];
    [task setArguments: arguments];

    // 新建输出管道作为Task的输出
    NSPipe *pipe = [NSPipe pipe];
    [task setStandardOutput: pipe];

    // 开始task
    NSFileHandle *file = [pipe fileHandleForReading];
    [task launch];

    // 获取运行结果
    NSData *data = [file readDataToEndOfFile];
    return [[NSString alloc] initWithData: data encoding: NSUTF8StringEncoding];
}
```

调用

```
错误示例：
[self cmd:@"cd Desktop"];
[self cmd:@"mkdir helloWorld"];
//这种调用方式结果是错误的，因为一条命令执行完Task就会销毁，相当于输入完终端关闭，再打开再输出，这时执行第二条语句时第一条语句已经不起作用了

//正确做法： 应使用下面这种方式实现
[self cmd:@"cd Desktop; mkdir helloWorld"];
```



1.推荐使用Taskit

```shell
Taskit *task = [Taskit task];
task.launchPath = @”/bin/echo”;
[task.arguments addObject:@”Hello World”];
task.receivedOutputString = ^void(NSString *output) {
NSLog(@”%@”, output);
};

[task launch];
```

优秀第三方库推荐

https://github.com/atg/taskit

https://github.com/reliablehosting/GCDTask

https://github.com/clickontyler/COTTaskHelper

https://github.com/MiCHiLU/Supervisor-NSTask

授权

https://github.com/djui/iLeopard/blob/master/iLeopard/NSTask%2BOneLineTasksWithOutput.m

参考资料

1、/bin、/sbin、/usr/bin、/usr/sbin目录的区别

http://www.361way.com/bin/1112.html

https://developer.apple.com/documentation/foundation/nstask

https://www.raywenderlich.com/125071/nstask-tutorial-os-x



https://devhub.io/zh/repos/XieXieZhongxi-AsyncTask