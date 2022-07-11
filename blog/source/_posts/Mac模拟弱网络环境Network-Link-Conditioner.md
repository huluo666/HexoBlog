---
title: Mac 模拟弱网络环境Network Link Conditioner
date: 2016-04-19 09:42:05
tags: iOS
grammar_cjkRuby: true
---

工具:`Network Link Conditioner`

下载地址:[https://developer.apple.com/downloads/?q=Hardware IO Tools ](https://developer.apple.com/downloads/?q=Hardware%20IO%20Tools)

`Network Link Conditioner` 是Xcode实用小工具,可以模拟多种网络环境，它内置以下几种网络环境配置:

```wiki
- EDGE
- 3G
- DSL
- WiFi
- High Latency DNS
- Very Bad Network
- 100% Loss
```

每种情况都是通过设置上载、下载的 [带宽](http://en.wikipedia.org/wiki/Bandwidth_%28computing%29), [延迟](http://en.wikipedia.org/wiki/Latency_%28engineering%29%23Communication_latency), 和 [丢包](http://en.wikipedia.org/wiki/Packet_loss)率 (如果设置为 0, 即不影响你当前的网络环境）

其它：CCWANem（来源：测试社区http://www.diggerplus.org/）

http://nshipster.cn/network-link-conditioner/

http://www.jianshu.com/p/367915734ad8