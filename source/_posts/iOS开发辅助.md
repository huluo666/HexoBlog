---
title: iOS开发辅助
tags:
categories:
date: 2017-03-30 15:17:51
grammar_cjkRuby: true
---

### 一、调色板增强扩展工具-skalacolor

skalacolor是系统调色板工具colorPicker的增强扩展，与系统数码测色计相比，skalacolor除了能拷贝十进制，十六进制颜色值外，还可以直接拷贝objective c、Swift、css颜色代码。

下载安装

https://bjango.com/mac/skalacolor/

如何使用

打开RTF文本编辑,然后按 **⇧⌘C **或选择 **格式→字体→显示颜色 **

使用帮助

https://bjango.com/help/skalacolor/gettingstarted/

![http://7xr7vj.com1.z0.glb.clouddn.com/UC20180314_105620.png](http://7xr7vj.com1.z0.glb.clouddn.com/UC20180314_105620.png)



### 二、颜色配置模板生成工具-ColorTools

**ColorTools**

https://github.com/ramonpoca/ColorTools

 Mac开发人员的颜色工具

用于从Mac ColorPicker调色板（.CLR）导入和导出颜色的小工具。

功能简介

- Adobe Swatch Exchange（.ASE）导入和导出。
- 十六进制颜色列表到系统调色板。
- 从颜色列表生成简单的UIColor类别。
- 从.xib / .storyboard进行颜色搜索和替换

ColorTools下载 https://github.com/ramonpoca/ColorTools/releases

用法：

```shell
$ cd ColorTools 			#进入Ase2Clr，Clr2Ase命令行工具目录
$ ./Ase2Clr FileName.ase  	 #执行Ase2Clr，将ase转成Mac调色板模板clr文件，生成Filename.clr在相同目录
```

```
./Ase2Clr FileName.ase -i
```

将生成的文件安装到`~/Library/Colors`中。您可能需要重新打开colorpicker才能刷新颜色列表。

**Html2Clr**

此工具读取包含十六进制编码颜色列表的文件，并为ColorPicker输出.clr文件，或将该列表作为新调色板安装在系统颜色选择器中。

输入格式是`#RRGGBB`颜色，后跟它们的名称，用空格分隔，每行一个

```
#fa3ada Almost-Magenta
#cada32 Green-Mustard
#ff6347 Tomato
...
```

用法类似于Ase2Clr：

```
./Html2Clr File.txt 			#当前路径	
./Html2Clr File.txt -i			#安装到~/Library/Colors中
```

**Clr2Obj**

该工具通过.clr文件创建UIColor（iOS）类别文件

```shell
./Clr2Obj File.clr AppColor #AppColor为CategoryName
```

**xibcolor**

这是一个python脚本，用于搜索和替换xib / storyboard文件中的颜色（只有xml格式）。

*注意*：这会覆盖文件！先备份你的文件（或让它在版本控制中提交）。

用法：

```
xibcolor file [-l|replacements|["#fromcolor" "#tocolor"]]
  file          a .storyboard or xib in new XML format
  -l            list colors present in the file
  replacements  a file consisting of pairs of hex-coded from->to colors
  #color        a pair of colors to replace
```

