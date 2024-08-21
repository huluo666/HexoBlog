---
title: shell自动打包IPA
date: 2017-03-19 17:48:40
tags: iOS
grammar_cjkRuby: true
---

自动打包ipa，测试发现时间和用xcode打包差不多，但可以减少鼠标点击操作。

```shell
#!/bin/bash

#----------使用说明---
#将EnterpriseExportOptionsPlist放在.xcodeproj同级目录
#运行脚本，直接拖放项目目录（.xcodeproj上级文件夹）
#mac 下终端访问文件出现“Permission Denied”解决方案：chmod a+x ./文件名
#----------使用说明 END---

#配置信息--Start---------
version_string="3.5.0"   	#版本号-空值不设置
build_number="3.5.1286" 	#版本号-空值不设置

##企业(enterprise)证书名&描述文件
ENTERPRISECODE_SIGN_IDENTITY="iPhone Distribution: xxx Service Co., Ltd."
EnterpriseBundleID="com.xxx.inhouse"
ENTERPRISEROVISIONING_PROFILE_NAME="73c2dfe3-ebc4-4801-bbd6-e4ba25f14fe9"


echo "请拖入项目文件目录(.xcodeproj文件的上层目录)"
# 读取用户输入并存到变量里
read user_input
Project_Path=$user_input

#EnterpriseExportOptionsPlist文件路径
EnterpriseExportOptionsPlist=$Project_Path/EnterpriseExportOptionsPlist.plist



echo "$EnterpriseExportOptionsPlist" 
echo "$Project_Path" 
cd $Project_Path

#查找项目名称
Project_Name=`find . -name *.xcodeproj | awk -F "[/.]" '{print $(NF-1)}'`

###############获取版本号,bundleID
info_plist=$Project_Path/$Project_Name/Info.plist
echo "~~~0~$Project_Name~~~~~"
ipa_outPath=~/Desktop/$Project_Name    #保存ipa文件到桌面

echo "~~~~~开始编译~~~~~"

#修改plist 版本号，修改build号函数--放在调用前
PlistBuddy_setting(){
	echo "~~~~~修改plist版本号~~~~~"
	bundleVersion=`/usr/libexec/PlistBuddy -c "Print CFBundleShortVersionString" $info_plist`
	bundleIdentifier=`/usr/libexec/PlistBuddy -c "Print CFBundleIdentifier" $info_plist`
	bundleBuildVersion=`/usr/libexec/PlistBuddy -c "Print CFBundleVersion" $info_plist`
	echo "~~~工程名~~~~~$Project_Name~~Bundle ID~~~$bundleIdentifier~~版本号~~~~~$bundleBuildVersion"

	if [ ! -n $version_string ]; then
		 echo "IS NULL"
		else
		# 修改版本号
		/usr/libexec/PlistBuddy -c "Set :CFBundleShortVersionString $version_string" "${info_plist}"
	fi

	if [ ! -n $version_string ]; then
		 echo "IS NULL"
		else
		# 修改build号
		/usr/libexec/PlistBuddy -c "Set :CFBundleVersion $build_number" ${info_plist}
	fi
}


xcodebuild_Archive(){
xcodebuild -project $Project_Name.xcodeproj -scheme $Project_Name -configuration Release -archivePath $Project_Name-enterprise.xcarchive clean archive build CODE_SIGN_IDENTITY="${ENTERPRISECODE_SIGN_IDENTITY}" PROVISIONING_PROFILE="${ENTERPRISEROVISIONING_PROFILE_NAME}" PRODUCT_BUNDLE_IDENTIFIER="${EnterpriseBundleID}"
}

xcodebuild_ExportArchive(){
xcodebuild -exportArchive -archivePath $Project_Name-enterprise.xcarchive -exportOptionsPlist $EnterpriseExportOptionsPlist -exportPath $ipa_outPath
}

#清理工程
xcodebuild -project ${Project_Path}/${Project_Name}.xcodeproj clean

#利用PlistBuddy修改plist信息
PlistBuddy_setting;

#企业打包脚本
xcodebuild_Archive;			#第一步--archive-.xcarchive
xcodebuild_ExportArchive;	#第二步--exportArchive-ipa

if [ -d "$ipa_outPath" ]; then
	echo "导出成功✅！！ $ipa_outPath" 
	open $ipa_outPath
fi
```