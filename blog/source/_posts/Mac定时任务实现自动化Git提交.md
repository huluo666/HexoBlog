---
title: Mac定时任务实现自动化Git提交
date: 2022-07-12 11:17:53
tags:
categorys:
---

## OSX系统添加定时任务

Mac 有两种方式可以添加定时任务：

1、launchctl 定时任务(推荐)

2、crontab命令

由于crontab可能会遇到用户交互权限问题导致execution error，plist对于MAC上系统交互的操作更支持，plist可以设置到秒，而crontab只能到分钟等原因，故选择使用plist方式实现。

进入`~/Library/LaunchAgents`，创建一个plist文件`com.github.autocommit.plist`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>com.git.autosync</string>
	<key>ProgramArguments</key>
	<array>
		<string>/Users/demo/run.sh</string>
	</array>
	<key>StartCalendarInterval</key>
	<dict>
		<key>Minute</key>
		<integer>45</integer>
		<key>Hour</key>
		<integer>11</integer>
	</dict>
</dict>
</plist>
```

### 3. 加载命令

```
launchctl load -w com.git.autosync.plist
```



```
// 加载任务
$ launchctl load -w com.git.autosync.plist

// 删除任务
$ launchctl unload com.git.autosync.plist

// 查看任务列表, 使用 grep '任务部分名字' 过滤
$ launchctl list | grep 'com.git.autosync'

// 开始
$ launchctl start com.git.autosync.plist

// 停止-停止任务。如果将任务已经是运行状态，则作业可能会立即重新启动。
$ launchctl stop com.git.autosync.plist
```



```sh
launchctl remove：从launchd中异步删除任务，在返回之前它不会等待作业实际停止，因此不会对任何错误做处理
launchctl unload：停止并卸载任务，但该任务仍将在下次登录/重新启动时重新启动
launchctl unload -w <路径>：停止并卸载和禁用任务。该任务将不会在下次登录/重新启动时重新启动。

其他几个相关命令

launchctl stop：停止任务。如果将任务已经是运行状态，则作业可能会立即重新启动。
launchctl load <路径>：只要未“禁用”该任务，就加载并启动任务。
launchctl load -w <路径>：加载并启动任务，同时还将任务标记为“未禁用”。任务将在下次登录/重新启动时重新启动。
```

http://www.wu.run/2019/03/27/mac-launchctl-guidance/
