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

由于crontab可能会遇到用户交互权限问题导致execution error， crontab 来配置定时任务很容易出错。在 Mac 中，配置定时任务有更好的选择。plist对于MAC上系统交互的操作更支持，plist可以设置到秒，而crontab只能到分钟等原因，故选择使用plist方式实现。

### 2. 编写plist文件

launchctl 将根据plist文件的信息来启动任务。
plist脚本一般存放在以下目录：

- `/Library/LaunchDaemons` -->只要系统启动了，哪怕用户不登陆系统也会被执行
- `/Library/LaunchAgents` -->当用户登陆系统后才会被执行

更多的plist存放目录：

> ~/Library/LaunchAgents 由用户自己定义的任务项
> /Library/LaunchAgents 由管理员为用户定义的任务项
> /Library/LaunchDaemons 由管理员定义的守护进程任务项
> /System/Library/LaunchAgents 由Mac OS X为用户定义的任务项
> /System/Library/LaunchDaemons 由Mac OS X定义的守护进程任务项

我们随便点看查看一些其中的任务配置：



```xml
<?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN"
        "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>Label</key>
        <string>org.getlantern</string>
        <key>ProgramArguments</key>
        <array>
        <string>/Applications/Lantern.app/Contents/MacOS/lantern</string>
        <string>-startup</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
    </dict>
    </plist>
```

解读：

`Label` 对应的 `org.getlantern` 表示任务名称，必须唯一。
 `ProgramArguments` 表示程序参数，数组的形式，第一个为 需要执行的程序或者脚本等，这里的 `/Applications/Lantern.app/Contents/MacOS/lantern` 和 `-startup` 意味着打开程序 `lantern` 。
 `RunAtLoad` 是个布尔值，表示开启自启项。

因此，该任务配置表示：设置 lantern 应用为开机自起项。

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





```
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">  
<plist version="1.0">  
  <dict>
    <!-- 名称，要全局唯一 -->
    <key>Label</key>
    <string>com.uniflor.notifier</string>

    <!-- 要运行的程序， 如果省略这个选项，会把ProgramArguments的第一个
    元素作为要运行的程序 -->
    <key>Program</key>
    <string>/Users/uniflor/script.sh</string>

    <!-- 命令， 第一个为命令，其它为参数-->
    <key>ProgramArguments</key>
    <array>
      <string>/Users/uniflor/script.sh</string>
    </array>

    <!-- 运行时间 -->
    <key>StartCalendarInterval</key>
    <dict>

      <key>Minute</key>
      <integer>30</integer>

      <key>Hour</key>
      <integer>9</integer>

      <key>Day</key>
      <integer>1</integer>

      <key>Month</key>
      <integer>5</integer>

      <!-- 0和7都指星期天 -->
      <key>Weekday</key>
      <integer>0</integer>

    </dict>

    <!-- 运行间隔，与StartCalenderInterval使用其一，单位为秒 -->
    <key>StartInterval</key>
    <integer>30</integer>

    <!-- 标准输入文件 -->
    <key>StandardInPath</key>
    <string>/Users/uniflor/run-in.log</string>

    <!-- 标准输出文件 -->
    <key>StandardOutPath</key>
    <string>/Users/uniflor/Bin/run-out.log</string>

    <!-- 标准错误输出文件 -->
    <key>StandardErrorPath</key>
    <string>/Users/uniflor/Bin/run-err.log</string>

  </dict>  
</plist>
```

> 1、Label：对应的需要保证全局唯一性；
>  2、Program：要运行的程序；
>  3、ProgramArguments：命令语句
>  4、StartCalendarInterval：运行的时间，单个时间点使用dict，多个时间点使用 array <dict>
>  5、StartInterval：时间间隔，与StartCalendarInterval使用其一，单位为秒，设置执行的时间间隔，单位为秒



多个时间任务

```xml
 <key>StartCalendarInterval</key>
    <array>
        <dict>
            <key>Weekday</key>  <!-- 周几 -->
            <integer>1</integer>
            <key>Hour</key>     <!-- 小时 -->
            <integer>8</integer>
            <key>Minute</key>   <!-- 分钟 -->
            <string>58</string>
        </dict>
        <dict>
            <key>Weekday</key>
            <integer>2</integer>
            <key>Hour</key>
            <integer>8</integer>
            <key>Minute</key>
            <string>52</string>
        </dict>
    </array>
```



示例

```xml
<!-- 这个表示每个小时的0分钟会执行此任务 -->
<key>StartCalendarInterval</key>
<dict>
  <key>Minute</key>
  <integer>0</integer>
</dict>
<!-- 在每天的3:55会执行此任务 -->
<key>StartCalendarInterval</key>
<dict>
  <key>Hour</key>
  <integer>3</integer>
  <key>Minute</key>
  <integer>55</integer>
</dict>
<!-- 在每六的3:15会执行此任务 -->
<key>StartCalendarInterval</key>
<dict>
  <key>Hour</key>
  <integer>3</integer>
  <key>Minute</key>
  <integer>15</integer>
  <key>Weekday</key>
  <integer>6</integer>
</dict>
```

3、验证脚本的正确性

你可以将执行时间设置为较近的时间，也可以使用下面语句直接执行一次脚本：



```cpp
// 开始
$ launchctl start com.test.task.plist
```

WorkingDirectory

```
          cmdStr: `${publicDirPath}/Autotask.py --bunldeid ${Bunlde_ID} --startinterval ${StartInterval} --scriptpath ${this.Script_Path}`

```

https://imchenway.com/2021/02/24/Mac%E8%87%AA%E5%AE%9A%E4%B9%89%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1/





## 注意事项

Python脚本不执行原因

1、无权限 

2、含有第三方库（如）
