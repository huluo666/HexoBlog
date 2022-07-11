---
title: 这可能是最快的打包IPA方式吧
tags: iOS
categories: iOS
date: 2017-04-25 14:03:10
grammar_cjkRuby: true
---

测试发现用脚本自动打包时间和用xcode正常的打包方式时间基本一样，只是少了些手动点击步骤。而采用以下方法时间大大缩小。

##### 第一步、使用iTunes将app打包成ipa

![](http://7xr7vj.com1.z0.glb.clouddn.com/UC20170425_101846.png)

##### 第二步、使用重签名工具重签名

https://github.com/DanTheMan827/ios-app-signer

![iregin](http://7xr7vj.com1.z0.glb.clouddn.com/UC20170425_102307.png)

​        当然然每次拖到iTunes里面多少有点麻烦，也可以在project下的`Build Phase`下`Add Run Script`添加一下shell脚本代码，这样每编译都会在`$PRODUCT_NAME.app`同级目录下生成一个`$PRODUCT_NAME.ipa`文件

```shell
/usr/bin/xcrun -sdk iphoneos PackageApplication -v "$BUILT_PRODUCTS_DIR/$PRODUCT_NAME.app" -o "$BUILT_PRODUCTS_DIR/$PRODUCT_NAME.ipa"
```



Cocoa开发之获取Keychain证书列表

http://www.skyfox.org/cocoa-keychain-certificate-list.html

获取 .mobileprovisioning profile with objectivec

http://stackoverflow.com/questions/19201040/read-mobileprovisioning-profile-with-objectivec



http://stackoverflow.com/questions/18849727/find-provisioning-profile-in-xcode-5