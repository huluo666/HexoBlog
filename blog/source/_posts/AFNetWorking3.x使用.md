---
title: AFNetworking3x使用
date: 2018-03-02 09:22:51
tags:
categorys: 工具
---

```objectivec
NSURLRequestUseProtocolCachePolicy： 对特定的 URL 请求使用网络协议中实现的缓存逻辑。这是默认的策略。
NSURLRequestReloadIgnoringLocalCacheData：数据需要从原始地址加载。不使用现有缓存。
NSURLRequestReloadIgnoringLocalAndRemoteCacheData：不仅忽略本地缓存，同时也忽略代理服务器或其他中间介质目前已有的、协议允许的缓存。
NSURLRequestReturnCacheDataElseLoad：无论缓存是否过期，先使用本地缓存数据。如果缓存中没有请求所对应的数据，那么从原始地址加载数据。
NSURLRequestReturnCacheDataDontLoad：无论缓存是否过期，先使用本地缓存数据。如果缓存中没有请求所对应的数据，那么放弃从原始地址加载数据，请求视为失败（即：“离线”模式）。
NSURLRequestReloadRevalidatingCacheData：从原始地址确认缓存数据的合法性后，缓存数据就可以使用，否则从原始地址加载
```

[iOS 使用AFNetWorking监听APP网络状态变化（可用于更改缓存策略、提示网络等）](http://blog.csdn.net/ll845876425/article/details/51086110)

[AFNetworking简单封装](https://bettermrli.com/2016/08/10/%E7%BD%91%E7%BB%9C/AFNetworking%E7%AE%80%E5%8D%95%E5%B0%81%E8%A3%85/)

http://flyweights.top/2015/09/23/AFNetworking%E6%93%8D%E4%BD%9C%E9%98%9F%E5%88%97%E8%AF%B7%E6%B1%82/

http://www.jianshu.com/p/8a7594df4659

https://github.com/Meeca/AFNDemo/tree/master/AFNetworkingDemo

```objectivec
1 ---- 查看 request 是否被缓存 --
NSCachedURLResponse * response = [cache cachedResponseForRequest:request];

     if (response != nil) {

         // 这里可以根据 是否有缓存，来更改缓存策略 --
         [request setCachePolicy:NSURLRequestReturnCacheDataDontLoad];
         NSLog(@"有缓存");
     }

2 -----
// 数据从网络加载完成，根据相应的缓存策略，需要本地缓存，

走 - (NSCachedURLResponse *)connection:(NSURLConnection *)connection willCacheResponse:(NSCachedURLResponse *)cachedResponse
--------------------

如果你不想过多的操作，可以直接返回 cachedResponse --
除非你不想缓存数据 -- 可以 return nil --

- (NSCachedURLResponse *)connection:(NSURLConnection *)connection willCacheResponse:(NSCachedURLResponse *)cachedResponse
{
    NSLog(@"cache response uerInfo== %@ \n response == %@  ", [cachedResponse userInfo], [cachedResponse response]);

    // 我们可以通过 转换 data ，来直接查看 缓存的内容额 --
    NSString * str = [[NSString alloc] initWithData:[cachedResponse data]
                                           encoding:NSUTF8StringEncoding];
    NSLog(@"data == %@", str);

    // 1 --------------

    // 一般情况下，我们并不对该方法进行操作 --
    return cachedResponse;


    // 2 -------------
    // 试着改一下缓存策略 -

    NSMutableDictionary * userInfo = [[cachedResponse userInfo] mutableCopy];
    NSMutableData * data = [[cachedResponse userInfo] mutableCopy];
    NSURLCacheStoragePolicy storagePolicy = NSURLCacheStorageNotAllowed;

    // NSURLCacheStorageAllowed, 缓存不受限制
    // NSURLCacheStorageAllowedInMemoryOnly, 只允许内存缓存，不允许磁盘缓存 -
    // NSURLCacheStorageNotAllowed 不缓存

    NSCachedURLResponse * response = [[NSCachedURLResponse alloc]
                                      initWithResponse:[cachedResponse response]
                                      data:data
                                      userInfo:userInfo
                                      storagePolicy:storagePolicy];
    return response;

    // 3 ----------------

    // 除非你不想缓存数据 -- 可以 return nil --
    return nil;

}
```

http://blog.originate.com/blog/2014/02/20/afimagecache-vs-nsurlcache/

http://blog.csdn.net/mengxiangone/article/details/48623785