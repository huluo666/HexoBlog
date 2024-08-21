---
title: Mac隐藏/显示桌面图标
date: 2016-05-12 16:36:42
tags: iOS
grammar_cjkRuby: true
---

//隐藏

```
defaults write com.apple.finder CreateDesktop -bool FALSE;killall Finder
```

//显示

```
defaults delete com.apple.finder CreateDesktop;killall Finder
```



实用工具-AppleScript编辑器

```
display dialog "桌面图标设置为可见或隐藏?" buttons {"可见", "隐藏"} with icon 2 with title "Switch to presentation mode" default button 1
set switch to button returned of result
if switch is "隐藏" then
	do shell script "defaults write com.apple.finder CreateDesktop -bool FALSE;killall Finder"
else
	do shell script "defaults delete com.apple.finder CreateDesktop;killall Finder"
end if
```


显示隐藏文件/文件夹

```
//显示
defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder

//隐藏
defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder

 //OS X Mountain Lion 
 defaults write com.apple.finder AppleShowAllFiles TRUE ; killall Finder
 defaults write com.apple.finder AppleShowAllFiles FALSE ; killall Finder

```


shell脚本

```
STATUS=`defaults read com.apple.finder AppleShowAllFiles` 
if [ $STATUS == YES ]; 
then 
defaults write com.apple.finder AppleShowAllFiles NO 
else 
defaults write com.apple.finder AppleShowAllFiles YES 
fi 
killall Finderß
```