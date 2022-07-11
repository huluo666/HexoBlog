---
title: 图片EXIF信息查看与Exiftool使用
date: 2018-03-02 09:22:51
tags:
categorys: 工具
---

​	今天元宵节，一位版主在论坛发了一篇元宵节祝福帖，一大早就有用户报障说帖子中有张图片看不了，安卓端正常，第一反应是图片格式问题，用Safari打开也看不了，回到公司证实了是一张webp格式图片，最后将其转换成jpg格式发给了用户重新上传替换。

下图后缀是jpg，实际是webp，在谷歌系浏览器上才显示正常

![76572794](http://7xr7vj.com1.z0.glb.clouddn.com/76572794.jpg)

### 一、什么是EXIF

​     可交换图像文件格式常被简称为Exif（Exchangeable image file format），是专门为数码相机的照片设定的，可以记录数码照片的属性信息和拍摄数据。Exif 可以被附加在 JPEG、TIFF、RIFF 等文件之中，为其增加有关数码相机拍摄信息的内容和缩略图或图像处理软件的一些版本信息。

了解更多：[维基百科](https://zh.wikipedia.org/wiki/EXIF)

##### EXIF在线查看

[图虫EXIF查看器alpha版](https://exif.tuchong.com/)

[exif-viewer](http://ganquan.info/exif-viewer/)

[get-metadata](https://www.get-metadata.com/ ) (支持图片、其它文件)

http://exif.regex.info/exif.cgi 支持URL链接

以上几个是使用过的对图片EXIF解析支持最好的，网上的一些工具会解析失败。



### 二、Exiftool使用

#### 一）为什么使用Exiftool

- `exiftool`是经过测试发现对图片EXIF信息解析支持是最好的（如果有更好的请补充）
- `exiftool`支持图片EXIF信息查询，修改及批量操作，还支持其它文件的EXIF操作。
- 安全隐私问题，因为经常有网上暴露图片隐私问题，使用在线需要上传到服务器。采用`exiftool`保证了图片的安全和隐私，显然是最适合的。

​	我们知道文件后缀名并不能代表文件的类型格式，比如上图webp格式后缀名是jpg，换言之一张jpg后缀名图片可能不是jpg图片，可能是`web`，`png`，`text`或其它类型文件。那么怎么快速了解它是哪种类型文件并获取它的一些其它信息呢，这时`exiftool`就派上用场了，试了下其它几个Exif工具，也用Mac/iOS原生代码测试了下，发现不能有效的读取jpg后缀的webp图片，而exiftool能很好支持。注意并非每一张图片都包含 exif 信息。像微信朋友圈如果发表的不是原图就没有。

##### 二）Exiftool安装与使用

##### 1、exiftool安装

```shell
$ brew install exiftool
```

或直接下载：https://sno.phy.queensu.ca/~phil/exiftool/

##### 2、查看EXIF信息

```shell
$ exiftool photo.jpg  #默认显示所有的信息 exiftool+图片路径/图片名.jpg
```

输出信息如下：

```shell
#$ ExifTool /Users/apple/Desktop/photo.jpg
ExifTool Version Number         : 10.80
File Name                       : photo.jpg
Directory                       : /Users/pconline/Desktop
File Size                       : 18 kB
File Modification Date/Time     : 2018:03:02 10:22:03+08:00
File Access Date/Time           : 2018:03:02 11:47:58+08:00
File Inode Change Date/Time     : 2018:03:02 11:47:57+08:00
File Permissions                : rw-r--r--
File Type                       : WEBP
File Type Extension             : webp
MIME Type                       : image/webp
VP8 Version                     : 0 (bicubic reconstruction, normal loop)
Image Width                     : 640
Horizontal Scale                : 0
Image Height                    : 400
Vertical Scale                  : 0
Image Size                      : 640x400
Megapixels                      : 0.256
```

##### 3、Exiftool常用命令示例

终端输入

```shell
$ exiftool -h #查看使用详细列表
```

使用帮助，可看到所有命令帮助，也可看到exiftool的支持文件类型和文件元信息如下：

```shell
File Types
----------------------+----------------------+---------------------
3G2   r/w   | DVB   r/w   | JPEG  r/w   | ODT   r     | RIFF  r
....
Meta Information
----------------------+----------------------+---------------------
EXIF           r/w/c  |  CIFF           r/w  |  Ricoh RMETA    r
GPS            r/w/c  |  AFCP           r/w  |  Picture Info   r
IPTC           r/w/c  |  Kodak Meta     r/w  |  Adobe APP14    r
XMP            r/w/c  |  FotoStation    r/w  |  MPF            r
MakerNotes     r/w/c  |  PhotoMechanic  r/w  |  Stim           r
Photoshop IRB  r/w/c  |  JPEG 2000      r    |  DPX            r
ICC Profile    r/w/c  |  DICOM          r    |  APE            r
MIE            r/w/c  |  Flash          r    |  Vorbis         r
JFIF           r/w/c  |  FlashPix       r    |  SPIFF          r
Ducky APP12    r/w/c  |  QuickTime      r    |  DjVu           r
PDF            r/w/c  |  Matroska       r    |  M2TS           r
PNG            r/w/c  |  MXF            r    |  PE/COFF        r
Canon VRD      r/w/c  |  PrintIM        r    |  AVCHD          r
Nikon Capture  r/w/c  |  FLAC           r    |  ZIP            r
GeoTIFF        r/w/c  |  ID3            r    |  (and more)
```

##### 4、查询相关命令

```shell
$cd /Users/apple/Desktop
exiftool photo.jpg #查看所有信息
exiftool -a -u -g1 photo.jpg #查看所有元信息，包括重复和未知标签，并按小组排列
exiftool -s -ImageSize -ExposureTime photo.jpg #查看图片尺寸
exiftool -common dir  #查看dir目录文件信息（不仅仅是图片）
exiftool -l  c.jpg d.jpg  #从两个图像文件打印所有信息。
exiftool -l -canon c.jpg d.jpg	#从两个图像文件打印标准的佳能信息。
```

##### 5、写入编辑命令

更改创建和修改时间

```shell
$ exiftool -gps:all= photo.jpg  #有些相机会记录拍照时的GPS定位信息。如果你不希望别人看到使用该命令删除gps信息
$ exiftool -all= photo.jpg  #删除所有信息
$ exiftool -all= --exif:all photo.jpg  #删除EXIF以外的所有信息
```

写入标签

```shell
$ exiftool -artist=标签名称 photo.jpg    		 #写入艺术家标签
$ exiftool -artist=标签名称 a.jpg b.jpg c.jpg   #写多个文件
$ exiftool -artist=标签名称  /exiftoolTest      #写在一个目录的所有文件 exiftoolTest为文件夹
```

其它：

```shell
exiftool -Comment ='这是一个新的评论'dst.jpg
    向JPG图片写入新评论（取代任何现有评论）。

exiftool -comment = -o newdir -ext jpg。
    删除当前目录中所有JPG图像的评论，
    将修改后的图像写入新目录。

exiftool -keywords = EXIF -keywords =编辑器dst.jpg
    用两个新的关键字（“EXIF”和。）替换现有的关键字列表
    “编辑”）。

exiftool -Keywords + =单词-o newfile.jpg src.jpg
    将源图像复制到新文件，然后添加关键字（“单词”）
    当前关键字列表。
            

exiftool -credit- = xxx dir
    从一个目录中的所有文件中删除信用信息，信用值是“xxx”。

exiftool -all = dst.jpg
    从图像中删除所有元信息。注意：你不应该
    对RAW图像（DNG除外）进行处理，因为它是专有的RAW图像
    格式通常包含制造注释中的信息
    转换图像所必需的。

exiftool -all = -comment ='寂寞'dst.jpg
    删除图像中的所有元信息并添加评论
    （注意顺序很重要：“-comment ='lonely'-all =”
    也会删除新评论。）

exiftool -all = --jfif：全部dst.jpg
    从图像中删除除JFIF组以外的所有元信息。

exiftool -Photoshop：全部= dst.jpg
    从图像中删除Photoshop元信息（注意
    Photoshop信息还包括IPTC）。

exiftool -r -XMP-crss：all = DIR
    递归删除a中的图像中的所有XMP-crss信息
    目录。

exiftool'-ThumbnailImage <= thumb.jpg'dst.jpg
    从指定的文件中设置缩略图（注意：引号是
    以防止外壳重定向）。

exiftool'-JpgFromRaw <=％d％f_JFR.JPG'-ext NEF -r。
    递归地写入以“_JFR.JPG”结尾的文件名的JPEG图像
    添加到扩展名为“.NEF”的类似文件的JpgFromRaw标记中
    当前目录。 （这是“-JpgFromRaw”的倒数
    上面的“READING EXAMPLES”部分的命令）。

exiftool -DateTimeOriginal - ='0：0：0 1：30：0'dir
    调整目录“dir”中所有图像的原始日期/时间
    减去1小时30分钟。 （这相当于
    “-DateTimeOriginal- = 1.5”。请参阅Image :: ExifTool :: Shift.pl
    细节。）

exiftool -createdate + = 3 -modifydate + = 3 a.jpg b.jpg
    向两个CreateDate和ModifyDate时间戳添加3个小时
    图片。

exiftool -AllDates + = 1：30 -if'$ make eq“Canon”'dir
    移动DateTimeOriginal，CreateDate和ModifyDate的值
    将所有佳能影像转换1小时30分钟
    目录。 （AllDates标签作为这些的快捷方式提供
    三个标签，允许他们通过一个标签访问。）

exiftool -xmp：city = Kingston image1.jpg image2.nef
    将标签写入两个图像的XMP组。 （没有“xmp：”
    自从“City”存在以后，该标签将被写入IPTC组
    在这两种情况下，IPTC默认是首选。）

exiftool -LightSource - ='未知（0）'dst.tiff
    只有在值为0时才删除“LightSource”标签。

exiftool -whitebalance- = auto -WhiteBalance = tung dst.jpg
    只有之前为“自动”时，才将“WhiteBalance”设置为“Tungsten”。

exiftool -comment- = -comment ='新评论'a.jpg
    只有当图片还没有时才写新评论。

exiftool -o％d％f.xmp目录
    为“dir”中的所有图像创建XMP元信息数据文件。

exiftool -o test.xmp -owner = Phil -title ='XMP File'
    仅从命令中定义的标签创建XMP数据文件
    线。
```

更多命令查看`exiftool -h`

http://owl.phy.queensu.ca/~phil/exiftool/examples.html



##### 6、使用ExifTool批量处理EXIF信息

如果要处理的文件太多，一张一张处理效率太低，ExifTool支持批量操作：

```shell
$ exiftool -artist=标签名称  /dirName    #批量写入dirName目录艺术家标签
$ exiftool -r -all= /dirName  	  #批量删除dirName及其子目录所有文件EXIF信息，-r表示递归处理子目录
$ exiftool -gps:all= /dirName 	  #批量删除dirName及其子目录所有文件gps信息
```



其它相关：

```shell
$ mdls  photo.jpg #这个命令显示指定文件的metadata的属性,当不能准确的获取exif信息
```

iOS端获取metadata

https://stackoverflow.com/questions/9766394/get-exif-data-from-uiimage-uiimagepickercontroller

https://stackoverflow.com/questions/16830180/how-to-get-exif-data-from-downloaded-image

测试发现，如果是网络webp图片,如果后缀是jpg或png不能准确获取Exif信息，系统相册没问题。

参考：

http://owl.phy.queensu.ca/~phil/exiftool/examples.html

https://zh.wikipedia.org/wiki/EXIF

https://baike.baidu.com/item/EXIF/422825

