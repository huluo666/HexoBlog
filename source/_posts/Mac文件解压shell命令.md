---
title: Mac文件解压shell命令
date: 2016-03-24 15:32:19
tags: iOS
grammar_cjkRuby: true
---

#### 一、文件解压相关格式
 Windows 平台-最常用的格式是 zip 和 rar，国内大多数是用 rar，国外大多数是用 zip。
类 Unix 平台而言-常用的格式是 tar 和 tar.gz，zip 比较少一些，rar 则几乎没有。
zip格式:Mac默认压缩格式，缺点:就是它的压缩率并不是很高，不如 rar及 tar.gz 等格式。

**ZIP**
压缩 zip 文档的命令如下：

```shell
zip -r archive_name.zip file_to_compress
zip -r archive_name.zip directory_to_compress/
```
解压 zip 文档的命令如下：

```shell
unzip archive_name.zip
```

**TAR**
tar 只是一种打包格式，并不对文件进行压缩，主要是为了便于文件的管理，所以打包后的文档大小一般远远大于 zip 和 tar.gz，优点:打包速度非常快，打包时 CPU 占用率低(因为不需要压缩嘛)。
将文件或文件夹打包为 tar 文档的命令如下：

```shell
tar -cvf archive_name.tar file_to_compress
tar -cvf archive_name.tar directory_to_compress
```
解包一个 tar 文档的命令如下：

```shell
tar -xvf archive_name.tar
```

**TAR.GZ**
tar.gz 可以说是对于 tar 的一个补充，它会对文件进行压缩，且压缩率略优于 zip，而对于 CPU 的占用率却不怎么高。Linux 平台下的大多数开源软件或源代码都是采用这种格式。
将文件或文件夹打包压缩为 tar.gz 文档的命令如下：

```shell
tar -zcvf archive_name.tar.gz file_to_compress
tar -zcvf archive_name.tar.gz directory_to_compress
```

解压一个 tar.gz 文档的命令如下：

```shell
tar -zxvf archive_name.tar.gz
```

**TAR.BZ2**
相比以上几种格式，tar.gz2 拥有最高的压缩率，但是压缩的时候所需要的时间也最长，CPU 占用率也最高。将文件或文件夹压缩为 tar.bz2 的命令如下：

```shell
tar -jcvf archive_name.tar.bz2 file_to_compress
tar -jcvf archive_name.tar.bz2 directory_to_compress
```

解压一个 tar.bz2 文件的命令是：

```shell
tar -jxvf archive_name.tar.bz2
```
**Com From**：https://libuchao.com/2013/04/21/linux-zip-tar-tar-gz-tar-bz2

#### tar命令详解
```shell
-c: 建立压缩档案
-x：解压
-t：查看内容
-r：向压缩归档文件末尾追加文件
-u：更新原压缩包中的文件
```

这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。
下面的参数是根据需要在压缩或解压档案时可选的。

```shell
-z：有gzip属性的,表明创建zip压缩文件，后面的后缀一定要是tar.gz
-j：有bz2属性的
-Z：有compress属性的
-v：显示所有过程
-O：将文件解开到标准输出
参数-f是必须的
-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。
```

**查看**
tar -tf aaa.tar.gz   			在不解压的情况下查看压缩包的内容

**压缩**
`tar –cvf jpg.tar *.jpg` 	//将目录里所有jpg文件打包成tar.jpg
`tar –czf jpg.tar.gz *.jpg` 	//将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
tar –cjf jpg.tar.bz2 *.jpg	 	//将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
tar –cZf jpg.tar.Z *.jpg   		//将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z

**二、gzip压缩文件的生成**
`tar -zcvf <压缩文件名>.tar.gz <要压缩的文件夹或者文件名>`

```shell
-z表明创建zip压缩文件，后面的后缀一定要是tar.gz
-c创建打包文件
-v显示压缩过程
-f归档名
```

文件分卷压缩
`split -b <分卷大小> <要拆分的文件名> <分卷名前缀>`
`split -b 900k test.tar.gz splt.tar.gz.`

将建立的test.tar.gz 拆分为数个大小不超过900k的文件
注意后面的 split.tar.gz. 以"."结尾的，这样拆分的文件就得到 split.tar.gz.aa split.tar.gz.ab....否则得到的是 xaa, xab, xac这样的文件
如果先建立一个压缩文件，再进行拆分的话，虽然可行，但是多少有些不方便。现在用 " | "通道将两个命令一同执行
`tar -zcvf - User_Guide.pdf  | split -b 900k - splt.tar.gz.`
如：

```shell
1、压缩
tar -zcf creatName.tar.gz living.jpg(单个文件)
tar –zcf creatName.tar.gz *.jpg (通配所有jpg文件)
2、分卷
split -b 900k creatName.tar.gz  splt.tar.gz.
生成名字splt.tar.gz.aa，splt.tar.gz.ab
3、解压
cat splt.tar.gz.a* | tar -jx
```

压缩framework

```shell
tar -cjf creatName.tar.bz2 *.framework
split -b 1500k creatName.tar.bz2  a.tx.libs.tar.bz2.
cat a.tx.libs.tar.bz2.a* | tar -jx
tar -jcvf - *.jpg  | split -b 100k - a.tx.libs.tar.bz2.
tar -jcvf - libs  | split -b 10000k - a.tx.libs.tar.bz2.
```

分卷压缩文件的合并
十分简单，用cat命令合并文件（cat也可用于文本文件的合并），用通配符指定要合并的文件即可
接上例。生成了 splt.tar.gz.aa ~~~~ splt.tar.gz.ad 共四个文件，同样将终端定位到桌面目录下复制内容到剪贴板代码:`cat split.tar.gz.a*>new.tar.gz`执行后即可看到桌面多出一个new.tar.gz 的压缩文件
类似3.当中提到的，这条合并命令一样可以用 | 在一条命令内实现“合并+解压”的任务。复制内容到剪贴板代码:`cat split.tar.gz.a* | tar -zxv`注意这里两步执行的时候同样用到了类似上面的缓存操作，所以并不需要指定合并后的压缩文件的具体名称，后面的tar命令也不需要加上-f参数指定名称了。
转自：http://mac.pcbeta.com/thread-112623-1-1.html
示例
使用 zip 命令压缩文件

```shell
zip - largefile | split -b 500k
```

将文件 *largefile* 压缩成 zip 包并分卷成不超过 500k 的文件，分解后文件名默认是 `x*` ，后缀为 2 位 `a-z` 字母，如 aa、ab。
要合并已分解的文件，可使用cat命令恢复成 *zip* 文件后使用 `unzip` 或其它主流解压软件解压：

```shell
cat x* > file.zip
```

tar 命令压缩文件

```shell
tar czvf - largefile | split -b 500k
```

使用 `tar` 解压：

```shell
cat x* | tar xzvf -
```

**相关压缩文件解压命令**
| 压缩文件格式       | 解压命令               |
| ------------ | ------------------ |
| *.tar        | tar –xvf           |
| *.gz         | gzip -d或者gunzip    |
| .tar.gz和.tgz | tar –xzf           |
| *.bz2        | bzip2 -d或者用bunzip2 |
| *.tar.bz2    | tar –xjf           |
| *.Z          | uncompress         |
| *.tar.Z      | tar –xZf           |