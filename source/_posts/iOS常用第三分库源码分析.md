---
title: iOS常用第三分库源码分析
date: 2018-03-27 13:55:14
tags:
categorys:
---

#### AFNetworking

GitHub:https://github.com/AFNetworking/AFNetworking

AF分为如下5个功能模块：

- 网络通信模块(AFURLSessionManager、AFHTTPSessionManger)
- 网络状态监听模块(Reachability)
- 网络通信安全策略模块(Security)
- 网络通信信息序列化/反序列化模块(Serialization)
- UIKit库的拓展：UIKit+AFNetworking

其中核心模块是AFURLSessionManager，也就是基于NSURLSessionManager封装的请求类。其余四个模块是为了配合网络通信而做的拓展类。AFHTTPSessionManger只是简单封装自NSURLSessionManager的类，简单的http请求一般就用这个类就足够了。

https://github.com/swiftcafex/NSURLSessionSamples

http://imtangqi.com/2016/04/01/from-nsurlconnection-to-nsurlsession/

http://imtangqi.com/2016/05/05/the-notes-of-learning-afnetworking-one/

https://renchao0711.github.io/2017/08/25/AFNetWorking%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90(%E4%B8%80)/



#### SDWebImage

GitHub:https://github.com/rs/SDWebImage

https://github.com/Draveness/analyze/blob/master/contents/SDWebImage/iOS%20%E6%BA%90%E4%BB%A3%E7%A0%81%E5%88%86%E6%9E%90%20---%20SDWebImage.md

http://xgfe.github.io/2017/05/27/shsoul/SDWebImage%E6%BA%90%E7%A0%81%E8%A7%A3%E8%AF%BB/

http://www.guiyongdong.com/2017/01/15/SDWebImage%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/

http://imtangqi.com/2016/03/19/the-notes-of-learning-sdwebimage-one/

http://www.chaisong.xyz/2016/01/25/SDWebImage%E6%BA%90%E7%A0%81%E7%A0%94%E7%A9%B6%E4%B8%8E%E5%AD%A6%E4%B9%A0/

