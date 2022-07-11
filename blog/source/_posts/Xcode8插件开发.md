---
title: Xcode8插件开发
date: 2017-06-05 12:08:40
tags:
categorys:
---

Xcode8以后, 以前的插件都不能用了。Apple官方推荐的方式*Xcode Source Editor Extension*来自己制作插件. Extension的方式开发的插件, 可以独立上架AppStore, 并且是独立于Xcode工程独立运行的. 但是没有UI交互, 不能在后台运行并且只能在开发者调用的时候直接修改代码。



![https://github.com/huluo666/NetWorkMonitorView/blob/master/NetWorkMonitorView/imge0.gif](https://github.com/huluo666/NetWorkMonitorView/blob/master/NetWorkMonitorView/imge0.gif)

官方文档

https://developer.apple.com/documentation/xcodekit

[Creating a Source Editor Extension](https://developer.apple.com/documentation/xcodekit/creating_a_source_editor_extension)

测试扩展

[Testing Your Source Editor Extension](https://developer.apple.com/documentation/xcodekit/testing_your_source_editor_extension)



#### Extension特性

- 支持上架到AppStore
- 每个Extension运行在独立的进程，如果它崩溃了，不会引起Xcode的崩溃，且会有错误提示



#### Extension缺陷

- 无UI交互
- 只能够在开发者调用相关命令的时候直接的修改代码，具体表现为 **获取正在编辑的文本** **获取选中的区域** **替换正在编辑的文本** **选中正在编辑的文本** **在Editor菜单中生成一个子菜单，用于调用插件** **绑定快捷键**
- Extension不能在后台运行





1、新建工程

打开Xcode-> `command + shift + N` ->选择macOS -> Cocoa Application, 点击Next新建一个工程。

2、新增Target（Extension）

在Xcode工具栏中选择`Editor -> Add Target -> MacOS -> Xcode Source Editor Extension-点击Activate`

新增后系统会默认创建`SourceEditorExtension`，`SourceEditorCommand`两个类。



 `invocation.commandIdentifier`; 命令标识符，对应plist文件中的`XCSourceEditorCommandIdentifier`字段，一个扩展可以有多个功能，用以区分需要执行的命令事件。

```
Identifier=com.pcauto.PCDevTools.AutoPropertyPlug.SourceEditorCommand
Identifier=com.pcauto.PCDevTools.AutoPropertyPlug.SourceEditorCommand2
```



```objectivec
#import "ViewController.h"

@interface ViewController()

@property (nonatomic, strong) NSString *name;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
}

@end
```



选择name属性行，点击插件生成属性

```objectivec
#import "ViewController.h"

@interface ViewController()

@property (nonatomic, strong) NSString *name;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
}

- (NSString *)name {
    if (!_name) {
        _name = [[NSString alloc] init];
    }
    return _name;
}

@end
```



`invocation.buffer.lines`是一个可变字符串数组，由于属性代码是倒序的，所以将属性代码依次插入到固定行即可，如果属性代码是顺序的，那么插入位置行数要++1

将属性代码插入到文本末尾

```objectivec
- (void)insertCodeWithStrings:(NSArray *)getterStrings {
      NSLog(@"getterStrings=%@",getterStrings);
    for (int i = 0; i < getterStringArray.count; i++) {
        NSLog(@"ViewController=%@-%zd",self.invocation.buffer.lines,self.endLineNumber);
        [self.invocation.buffer.lines insertObject:getterStrings[i] atIndex:self.endLineNumber];
    }
}
```



invocation.buffer.lines 所有行





#### 7.测试Extension

点击xcode工具栏->Editor->插件菜单



#### 8.绑定快捷键

1. 添加快捷键: Xcode -> “Preferences” -> “Key Bindings” -> 搜索插件名字 -> 添加对应的快捷键.

快捷键`command+，`

```
占位符的格式：<#param#> <#className#> <#objName#>

- (#className# *)#objName# {\n    if (!_#objName#) {\n        _#objName# = [[#className# alloc] init];\n    }\n    return _#objName#;\n}\n
```



系统偏好设置路径

```
/System/Library/PreferencePanes
/System/Library/PreferencePanes/Extensions.prefPane 扩展
```

问题备忘与解决方法

1、`There is a problem launching using posix_spawn (error code: 2).`

将Xcode加入到Scheme的执行菜单里

```
打开Xcode->Product->EditScheme->Run->Executable->Xcode.app
```

![](http://7xr7vj.com1.z0.glb.clouddn.com/UC20180306_092216.png)



```
IDEExtensionManager: Xcode Extension does not meet code signing requirement: Error Domain=DVTSecErrorDomain Code=-67050 "code failed to satisfy specified code requirement(s)" UserInfo={NSLocalizedDescription=code failed to satisfy specified code requirement(s)}
```

工程Target和Xcode Extension Target 需要选择一个签名证书 `Target->signing->team->签名证书`

