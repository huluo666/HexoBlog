---
title: 创建Xcode模板
date: 2016-05-23 14:32:37
tags: iOS
grammar_cjkRuby: true
---

#### 一、为什么要自定义模板
1.节省重复代码手写时间

2.统一规范代码，提高代码可读性
3.减少手写代码,XIB或修改相关配置等不必要的时间

如我们要求所有的viewController的代码都得按照一下代码结构来写:
```obj
#pragma mark - def
#pragma mark - override
#pragma mark - api
#pragma mark - model event 
#pragma mark - view event
#pragma mark - private
#pragma mark - getter / setter
```
[iOS代码编程规范-根据项目经验汇总](http://www.jianshu.com/p/08be5b30ff82)


#### 二、模板存放位置
Xcode模板主要分为2种，系统默认模板和用户自定模板，对iOS App开发者而言，一般用到的是系统模板中的`/Applications/Xcode.app/Contents/Developer/Platforms`目录下的`iPhoneOS.platform`中的模板

![1.png](http://upload-images.jianshu.io/upload_images/328273-4c4941288e786bfb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**1、Xcode系统模板位置**

(1) iOS开发系统模板位置
- 里面包含文件模板（File Templates）和工程模板（Project Templates）

```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates
```

(2) MacOSX的系统文件模板位置
```shell
/Applications/Xcode.app/Contents/Developer/Library/Xcode/Templates
```

**2.用户自定义模板位置**
 `~/Library/Developer/Xcode/Templates` 

终端命令打开文件目录
```shell
cd ~/Library/Developer/Xcode/Templates //进入目录
open .							     //打开目前目录
等价于
open ~/Library/Developer/Xcode/Templates
```

或者点击Finder菜单栏的`前往>前往文件夹`(`shift + command + G`) ，
输入：`~/Library/Developer/Xcode/Templates`

会看到`File Templates`，`Project Templates`2个文件夹，分别代表`文件模板`和`工程模板`目录。

#### 三、如何速创建、修改Xcode模板

 Xcode没有提供直接的工具或者是向导给你创建一个工程模板，,但是我们可以找到Xcode内置的几个模板,这里以iPhone开发为说明,介绍模板的创建修改过程.

**iOS模板目录**

`/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates`

终端命令打开方式

```shell
open /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates
```

或者点击Finder菜单栏的`前往>前往文件夹`(`shift + command + G`)

输入:
```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates
```

**1）基于现成模板修改或使用**

拷贝`iOS系统模板目录`中的模板进行修改。

为Xcode添加`Empty Application`、`category`、`protocol`等模板

现成下载：https://github.com/NSFish/AddMissingTemplates

​		   https://github.com/ChenYilong/XcodeMissingTemplates

也可以利用`AlcaAtraz`安装相关模板，`shift+command+9` >`Templates`模板

推荐：https://github.com/zubco/PZCustomView

 复制模板文件夹到用户自定义模板目录 `~/Library/Developer/Xcode/Templates/` ，重启即可

**2）完全自定义模板**
例如创建一个带有导航栏和标签栏控制器的工程
1、进入模板目录
`/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/Project\\ Templates/iOS`

2、手动创建`CustomTemplate`文件夹
![3.png](http://upload-images.jianshu.io/upload_images/328273-c013cfd53a4b00c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将原来XCode6中的“Empty Application”模板拷贝过来（可以从上文提到的Github中下载），修改增加一些必要字段。
Plist文件内容如下图
![2.png](http://upload-images.jianshu.io/upload_images/328273-f9cd3a40efa73169.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

PS:基于标签栏和导航栏工程模板文件相关内容详见文末[Github](https://github.com/huluo666/CustomTemplate)
相关字段含义查阅3.1

**3.1 Xcode模板 文件宏**

| 占位符                              | 意义                |
| :------------------------------- | :---------------- |
| `___FILENAME___`                 | 文件名包括后缀           |
| `___PROJECTNAME___`              | 当前工程名，在创建工程时设置的   |
| `___FULLUSERNAME___`             | 当前登录用户的名字         |
| `___DATE___`                     | 当前日期 ，格式为MM/DD/YY |
| `___FILEBASENAMEASIDENTIFIER___` | 不带后缀的文件名          |
| `___projectnameasidentifier___`  | 项目名称转换为有效的C风格的标识符 |
| `___organizationname___`         | 在Xcode项目定义的组织名称   |
| `___time___`                     | 当前时间              |
| `___year___`                     | 前四位数的年份           |

From:http://see.sl088.com/wiki/Xcode%E6%A8%A1%E6%9D%BF_%E6%96%87%E4%BB%B6%E5%AE%8F


**3.2文件组成**
--TemplateInfo.plist（`必要`）：所有的模板属性设置都在这里。
--TemplateIcon.tiff（`可选`）：定义显示在new project的dialog中的模板图标。
-- Main_iPhone.storyboard、Main_iPad.storyboard：要添加在项目中的文件。



**3.3TemplateInfo.plist字段详解**
- `Kind`（必须） 模板类型
  Xcode.Xcode3.ProjectTemplateUnitKind --指定该模板是工程(项目)模板
  Xcode.IDEFoundation.TextSubstitutionFileTemplateKind --指定该模板是文件模板
- `SortOrder` 这个是排序的值，该模板显示在new project的dialog中的位置索引,可以设置在界面中的摆放位置，值越大越前面
- `Ancestors`：要继承的模板。也就是模板的“父类”，从父类那里继承一些模板的基础属性，可以有多个父类。
- `Concrete`：设置为YES的模板才可以显示在new project的dialog中，此时这个模板不能被其他模板继承。
- `Description`：就是Description描述信息。
- `Identifier`：模板的唯一标示符，若模板B要继承模板A，就在模板B的Ancestors中写上模板A的Identifier。
- `Nodes`：定义要添加到项目中的文件，目标结构节点。
- `Definitions`：将Nodes中定义的文件添加到项目中（相关.h/.m文件）。
- `Options`：定义在new project中选择模板后点击next后的dialog中的内容，如Product Name、Organization Name、Company Identifier、Bundle Identifier等。
- 在Suffixes里面添加自定义的模板类的类名以及模板类所继承的类名



#### 四、其它应用实例

**iOS开发网络适配https，修改模板方式解决**
iOS9让所有的HTTP默认使用了HTTPS，App无法正常访问HTTP链接。
`1、常规解决方法`：
 [iOS9 HTTP 不能正常使用的解决办法](https://segmentfault.com/a/1190000002933776)
`2、Xcode模板修改步骤`
**步骤1、：进入工程模板目录或直接打开编辑**

```shell
open  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/Project\\ Templates/iOS/Application/Cocoa\\ Touch\\ Application\\ Base.xctemplate/TemplateInfo.plist
```

编辑`Cocoa Touch Application Base.xctemplate/TemplateInfo.plist`文件

**步骤2、添加key&value值**

- 在Nodes中增加一个item， 值（右侧）设置为Info.plist:NSAppTransportSecurity  。

- 在Definitions字段下增加item， 键（左侧）设置为Info.plist:NSAppTransportSecurity ，值（右侧）设置为

  ```
  <key>NSAppTransportSecurity</key>
  <dict>
  <key>NSAllowsArbitraryLoads</key>
  <true/>
  </dict>
  ```

![4.png](http://upload-images.jianshu.io/upload_images/328273-daa93a71cc9df9f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意第4点，里面内容不仅仅是所见的`NSAppTransportSecurity`，包含以上字典所有内容

`Github` **[CustomTemplate](https://github.com/huluo666/CustomTemplate)**

下载安装：
打开/ios文件目录，将`UITbaBar&Nav.xctemplate`模板放置在`CustomTemplate`目录,如图3所示
```
open /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/Project\\ Templates/iOS
```