---
title: shell 学习
date: 2016-03-18 17:33:45
tags: shell
grammar_cjkRuby: true
categories: [shell]
---

shell程序设计

http://opus.konghy.cn/shell-tutorial/index.html

`#!/bin/sh`

shell编程是以"#"为注释，但对`"#!/bin/sh"`却不是。

`"#!/bin/sh"`是对shell的声明，说明你所用的是那种类型的shell及其路径所在。

\#!/bin/sh 是指此脚本使用/bin/sh来解释执行，#!是特殊的表示符，其后面根的是此解释此脚本的shell的路径。

`“#!”`是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行。

C是编译型语言，而shell是解释型语言。


https://github.com/qinjx/30min_guides/blob/master/shell.md

```
#!/bin/sh
cd ~
mkdir shell_tut
cd shell_tut

for ((i=0; i<10; i++)); do
    touch test_$i.txt
done
```

### 示例解释

- 第1行：指定脚本解释器，这里是用/bin/sh做解释器的
- 第2行：切换到当前用户的home目录
- 第3行：创建一个目录shell_tut
- 第4行：切换到shell_tut目录
- 第5行：循环条件，一共循环10次
- 第6行：创建一个test_1…10.txt文件
- 第7行：循环体结束



**流程判断语句if/if else/if else-if else**

#### if

```
if condition
then
    command1
    command2
    ...
    commandN
fi
```



#### if else

```
if condition
then
    command1
    command2
    ...
    commandN
else
    command
fi
```



#### if else-if else

```
if condition1
then
    command1
elif condition2
    command2
else
    commandN
fi
```



写成一行用分号隔开（适用于终端命令提示符）：

```
if `ps -ef | grep ssh`;  then echo hello; fi
```

末尾的fi就是if倒过来拼写



### for while

#### for

```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

#### C风格的for

```
for (( EXP1; EXP2; EXP3 ))
do
    command1
    command2
    command3
done
```

#### while

```
while condition
do
    command
done
```

#### 无限循环

```
while :
do
    command
done
```

或者

```
while true
do
    command
done
```

或者

```
for (( ; ; ))
```

#### until

```
until condition
do
    command
done
```

### case

```
case "${opt}" in
    "Install-Puppet-Server" )
        install_master $1
        exit
    ;;

    "Install-Puppet-Client" )
        install_client $1
        exit
    ;;

    "Config-Puppet-Server" )
        config_puppet_master
        exit
    ;;

    "Config-Puppet-Client" )
        config_puppet_client
        exit
    ;;

    "Exit" )
        exit
    ;;

    * ) echo "Bad option, please choose again"
esac
```

case的语法和C family语言差别很大，它需要一个esac（就是case反过来）作为结束标记，每个case分支用右圆括号，用两个分号表示break





执行

```shell
$ /bin/sh  test.sh
$ /bin/php test.php

# Permission denied
$chmod +x test.sh	 #使脚本具有执行权限 OR chmod 755 test.sh
$./test.sh			 #执行脚本
```



关于 `man` 命令

相当于帮助命令，如输入`man ls`即可进入使用指南页面。



**常用命令文件夹操作命令**

| 目录参数 | 功能描述         | 事例                  |
| ---- | ------------ | :------------------ |
| .    | 表示当前目录       | cd .  ls            |
| ..   | 表示当前目录的上一级目录 | open .              |
| /    | 根目录/目录分隔符    | cd /                |
| ./   | 当前目录         |                     |
| ../  | 回到上一级目录      | cd ../..=cd ../+ .. |
| ~    | 当前登陆用户目录     |                     |

相对路径/绝对路径 凡是以/开始的路径，都是绝对路径



| 命令   | 功能描述        |                   |
| ---- | ----------- | ----------------- |
| pwd  | 显示当前目录的绝对路径 |                   |
| ls   | 列出当前目录的内容   |                   |
| cd   | 切换工作目录      | cd /Users/myhome/ |
| open | 打开当前目录      |                   |



### 处理特殊字符

如果目录中有特殊字符（空格，括号，引号，`[]`，`!`，`$`，`&`，`*`，`;`，`|`，`\`），那么直接输入空格会造成系统识别困难，必须使用特殊的语法来表示这些字符。例如上例中，空格前添加反斜杠“`\`”（back slash）即可：`cd Punlic/Drop\ Box/`。除了反斜杠，也可以用引号的方法：`cd "Public/Drop Box"。`



**shell命令ifconfig**

```shell
显示: ifconfig [设备名] 如 ifconfig en0
显示过滤: ifconfig [设备名] |grep [string] 如 ifconfig en0 |grep ether
配置ip: ifconfig [设备名] [ip] netmask [netmask]
配置硬件ID: ifconfig en0 hw ether [硬件地址]
启用或禁用设备 ifconfig en0 [up/down]

$ ifconfig | grep "inet"
```


###  目录操作

| 命令名    | 功能描述       | 使用举例                                  |
| :----- | :--------- | ------------------------------------- |
| mkdir  | 创建一个目录     | mkdir dirname                         |
| rmdir  | 删除一个目录     | rmdir dirname                         |
| mvdir  | 移动或重命名一个目录 | mvdir dir1 dir2                       |
| cd     | 改变当前目录     | cd dirname                            |
| pwd    | 显示当前目录的路径名 | pwd                                   |
| ls     | 显示当前目录的内容  | ls -la **-w 显示中文，-l 详细信息， -a 包括隐藏文件** |
| dircmp | 比较两个目录的内容  | dircmp dir1 dir2                      |



用terminal启动相应程序打开文件的方法

 `open (-a) [program][filename]`

```shell
$ open  -a Typora  README.md
```


**通配符**

| 通配符  | 作用                                       |
| ---- | ---------------------------------------- |
| ?    | 匹配一个任意字符                                 |
| *    | 匹配0个或任意多个任意字符，也就是可以匹配任何内容                |
| [ ]  | 匹配中括号中任意一个字符，例如：[abc]代表一个字符，或者a,或者b，或者c  |
| [-]  | 匹配中括号中任意一个字符，代表一个范围。例如：[a-z]代表匹配一个小写字母   |
| [^]  | 逻辑非，表示匹配不是中括号内的一个字符，例如：[^0-9]代表匹配一个不是数字的字符 |


shell权限修改

```shell
chmod 777 aa.sh
chmod -x aa.sh
```


http://c.biancheng.net/cpp/shell/ C语言中文网
[Mac shell入门](http://www.iosxxx.com/blog/2015-12-25-mac-shellru-men.html#u53C2_u8003_u76EE_u5F55)
[Mac 终端命令大全](http://www.jianshu.com/p/3291de46f3ff)
[Mac OS X 终端常用命令大全](http://www.weiosx.com/show-12-142-1.html)
[SHELL中的循环语句总结(FOR, WHILE, UNTIL）](http://www.iosxxx.com/blog/2015-12-25-mac-shellru-men.html#u53C2_u8003_u76EE_u5F55)
[Mac OS X Terminal 101：终端使用初级教程](http://www.renfei.org/blog/mac-os-x-terminal-101.html)
[Bash脚本获取自身路径方法](http://metman.info/blog/2014/11/05/bashjiao-ben-huo-qu-zi-shen-lu-jing-fang-fa/)
[MAC全栈开发环境搭建指南](http://aotu.io/mac/docs/)
[新编码神器Atom使用纪要](http://www.jeffjade.com/2016/03/03/2016-03-02-how-to-use-atom/)
http://w3cshow.com/2015/05/23/mac-terminal-md/
[使用shell脚本批量修改文件后缀名](http://www.jianshu.com/p/161a77e580b7)
[Linux Shell简明教程（一）](http://www.jellythink.com/archives/699)
http://www.runoob.com/linux/linux-shell.html



Shell参数解析脚本传参方法总结

http://www.jianshu.com/p/ff85119d2630
https://www.jianshu.com/p/d3cd36c97abc
http://www.zmonster.me/2014/08/09/pare-arguments-in-shell-function.html

shell脚本中echo显示内容带颜色的实现方法

http://www.jianshu.com/p/ba1b8aded634
http://huangshanben.com/articles/3031/





