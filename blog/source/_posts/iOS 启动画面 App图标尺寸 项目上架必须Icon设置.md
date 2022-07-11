---
title: iOS 启动画面 App图标尺寸 项目上架必须Icon设置
tags: Java
categories: Java
date: 2017-08-02 18:08:43
---

### **1.1AppIcon图标尺寸如下：**

**说明：AppIcon （6张） AppStore Icon （1张）（png格式）**

![071754406188491](http://7xr7vj.com1.z0.glb.clouddn.com/071754406188491.png)

**AppStore Icon** --- **1024x1024（必须）**

### **1.2启动画面LaunchImage图标尺寸如下：**

**说明：至少切下图中显示的4张尺寸（png格式）**

**如果需要适配ipad 请参考1.4**

![062255583214204](http://7xr7vj.com1.z0.glb.clouddn.com/062255583214204.png)

 

### 1.3提交至AppStore展示图**图片尺寸如下**

**说明：每个尺寸5张图片（3.5寸可忽略不切），一共15张张视图**

![062254400716177](http://7xr7vj.com1.z0.glb.clouddn.com/062254400716177.png)

### **参考尺寸及命名下:**

| 编号   | 尺寸          | 命名                                     |
| ---- | ----------- | -------------------------------------- |
| 1    | 640 × 1136  | LaunchImage-1-568h@2x.png              |
| 2    | 640 × 1136  | LaunchImage-1-700-568h@2x.png          |
| 3    | 640 × 960   | LaunchImage-1-700@2x.png               |
| 4    | 750 × 1334  | LaunchImage-1-800-667h@2x.png          |
| 5    | 1242 × 2208 | LaunchImage-1-800-Portrait-736h@3x.png |
| 6    | 640 × 960   | LaunchImage-1@2x.png                   |

  

**真机尺寸 **

| **设备**                    | **屏幕尺寸**    | **Reader** | **设计尺寸(px)** | **渲染后**   |
| ------------------------- | ----------- | ---------- | ------------ | --------- |
| iPhone 5/5s/5c            | 4.0（326PPI） | @2x        | 640x1136     |           |
| iPhone 6/6s/7/8           | 4.7（326PPI） | @2x        | 750x1134     |           |
| iPhone6 Plus/7 Plus/8Plus | 5.5（401PPI） | @3x        | 1242x2208    | 1080x1920 |
| iPhone X                  | 5.8（458PPI） | @3x        | 1125x2436    |           |

```
iPhone4(s)：分辨率960*640，高宽比1.5
iPhone5(s)：分辨率1136*640，高宽比1.775
iPhone6/7：分辨率1334*750，高宽比1.779
iPhone6+/7+：分辨率1920*1080，高宽比1.778
```

可粗略认为iPhone5(s)、6(+)的高宽比是一致的（16:9），即可以等比例缩放。因此可以按宽度适配：` fitScreenWidth= width*(SCREEN_WIDTH/320)`



 **1.4 如果需要适配iPad，还需要添加以下分辨率图片**

需要启动图尺寸:(横竖屏)

数据参考来源：

[【官网】iPhone 机型比较](https://www.apple.com/cn/iphone/compare/)

[The Ultimate Guide To iPhone Resolutions](https://www.paintcodeapp.com/news/ultimate-guide-to-iphone-resolutions)

http://www.ui001.com/chicun/