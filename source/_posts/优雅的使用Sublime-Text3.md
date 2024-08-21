---
title: 优雅的使用Sublime-Text3
date: 2018-03-29 10:26:51
tags:
categorys: [编辑器]
---

https://www.sublimetext.com/
http://www.sublimetextcn.com/

如何安装插件详见：[https://packagecontrol.io/installation](https://link.jianshu.com/?t=https://packagecontrol.io/installation)

安装方法：

`Cmd+Shift+P`->`install package` -> 输入主题名Theme

推荐使用[package Control](https://packagecontrol.io/)安装

Boxy主题安装

1、`Cmd+Shift+P->install package` -> Boxy Theme

2、`Cmd+Shift+P->install package` -> A File Icon

主题选择-`Preferences->Color Theme`

使用ColorSublime插件浏览主题

[ColorSublime](https://link.jianshu.com/?t=http://colorsublime.com/)：Sublime主题网站

[ColorSublime插件](https://link.jianshu.com/?t=https://github.com/Colorsublime/Colorsublime-Plugin) ：主题实时预览插件，推荐使用`Package Control`安装，安装完可以使用在控制面板中移动上下箭头就可以预览，回车即可安装。

使用：`Cmd+Shift+P`->`UI:Select Theme`

`esc`--退出当前操作

#### 主题推荐
Spacegray
ayu
Material
GitHub
Solarized
Soda

#### 插件推荐

SideBarEnhancements-左侧文件管理栏增强

Markdown Extended-让sublime text支持markdown，并支持预览。

Alignment —代码对齐

ColorPicker-调色板

TrailingSpaces-检测并一键去除代码中多余的空格

SublimeCodeIntel
代码提示和补全插件，支持 JavaScript、Mason、XBL、XUL、RHTML、SCSS、Python、HTML、Ruby、Python3、XML、Sass、XSLT、Django、HTML5、Perl、CSS、Twig、Less、Smarty、Node.js、Tcl、TemplateToolkit 和 PHP 等所有语言
SublimeLinter
一个支持lint语法的插件，可以高亮linter认为有错误的代码行，也支持高亮一些特别的注释
ChineseLocalization--汉化

插件推荐

http://www.cnblogs.com/hykun/p/sublimeText3.html

**卸载插件**

按`ctr+shift +p`然后输入remove 回车，再输入要删除的插件名

小技巧：点击文件进行编辑模式，再次点击失去编辑模式。这样键盘上下键可以快速浏览文件


#### 管理文档资料
一直用Typora写文档资料，但Typora没有文档管理功能，使用Sublime直接拖入文件夹即可。可以查看源码格式。

批量替换
hexo主题objective-c无法高亮，使用“objectivec”字段进行高亮
Find->Find in files（⌘+shift+F）--替换后记得保存所有->File-save-all(⌘+control+s)


**相关插件**
MarkdownEditing —语法高亮，快捷键

```shell
⌘+option+V 选中的内容将自动转换为行内式超链接，链接到剪贴板中的内容
⌘+option+R 选中的内容将自动转换为参考式超链接，链接到剪贴板中的内容
⌘+Shift+K 插入一个标准的行内式图片
```

Typora -用Typora打开插件
DeleteBlankLines-删除空行，选择需要删除空行的文本，⌘+shift+p，选择delete blank lines

```shell
⌘+Alt+Delete             删除所有空行
⌘+Alt+Shift+Delete       删除多余空行
```


在相同窗口打开文件

每次点击一个文件都会打开新的窗口，然后自己手动关闭

How to open files in the same window?

Anwer:select **Preferences -> Settings-User** and add

```
"open_files_in_new_window": false
```

这样单击不会再新窗口打开，双击新窗口打开文件

搜索：

- 用 `⌘ + P` 可以快速跳转到当前项目中的任意文件，可进行关键词匹配。
- 用 `⌘ + P` 后 `@` (或是`⌘ +R`)可以快速列出/跳转到某个函数


#### 快捷键

- **⌘**= Command key
- **⌃***=Control key
- **⌥**=Option key
- **⇧**=Shift Key

快捷键在线练习：https://www.shortcutfoo.com/app

```shell
⌘+Shift+P：打开命令面板
⌘+P：搜索项目中的文件
⌘+G：跳转到第几行
⌘+W：关闭当前打开文件
⌘+Shift+W：关闭所有打开文件
⌘+Shift+V：粘贴并格式化
⌘+D：选择单词，重复可增加选择下一个相同的单词
⌘+L：选择行，重复可依次增加选择下一行
⌘+Shift+L：选择多行
⌘+Shift+Enter：在当前行前插入新行
⌘+X：删除当前行
⌘+M：跳转到对应括号
⌘+U：软撤销，撤销光标位置
⌘+J：选择标签内容
⌘+F：查找内容
⌘+Shift+F：查找并替换
⌘+H：替换
⌘+R：前往 method
⌘+N：新建窗口
⌘+K+B：开关侧栏
⌘+Shift+M：选中当前括号内容，重复可选着括号本身
⌘+F2：设置/删除标记
⌘+/：注释当前行
⌘+Shift+/：当前位置插入注释
⌘+Alt+/：块注释，并Focus到首行，写注释说明用的
⌘+Shift+A：选择当前标签前后，修改标签用的
F11：全屏
Shift+F11：全屏免打扰模式，只编辑当前文件
Alt+F3：选择所有相同的词
Alt+.：闭合标签
Alt+Shift+数字：分屏显示
Alt+数字：切换打开第N个文件
Shift+右键拖动：光标多不，用来更改或插入列内容
鼠标的前进后退键可切换Tab文件
按⌘，依次点击或选取，可需要编辑的多个位置
按⌘+Shift+上下键，可替换行
```


# 选择类
⌘+D 选中光标所占的文本，继续操作则会选中下一个相同的文本。
Alt+F3 选中文本按下快捷键，即可一次性选择全部的相同文本进行同时编辑。举个栗子：快速选中并更改所有相同的变量名、函数名等。
⌘+L 选中整行，继续操作则继续选择下一行，效果和 Shift+↓ 效果一样。
⌘+Shift+L 先选中多行，再按下快捷键，会在每行行尾插入光标，即可同时编辑这些行。
⌘+Shift+M 选择括号内的内容（继续选择父括号）。举个栗子：快速选中删除函数中的代码，重写函数体代码或重写括号内里的内容。
⌘+M 光标移动至括号内结束或开始的位置。
⌘+Enter 在下一行插入新行。举个栗子：即使光标不在行尾，也能快速向下插入一行。
⌘+Shift+Enter 在上一行插入新行。举个栗子：即使光标不在行首，也能快速向上插入一行。
⌘+Shift+[ 选中代码，按下快捷键，折叠代码。
⌘+Shift+] 选中代码，按下快捷键，展开代码。
⌘+K+0 展开所有折叠代码。
⌘+← 向左单位性地移动光标，快速移动光标。
⌘+→ 向右单位性地移动光标，快速移动光标。
shift+↑ 向上选中多行。
shift+↓ 向下选中多行。
Shift+← 向左选中文本。
Shift+→ 向右选中文本。
⌘+Shift+← 向左单位性地选中文本。
⌘+Shift+→ 向右单位性地选中文本。
⌘+Shift+↑ 将光标所在行和上一行代码互换（将光标所在行插入到上一行之前）。
⌘+Shift+↓ 将光标所在行和下一行代码互换（将光标所在行插入到下一行之后）。
⌘+Alt+↑ 向上添加多行光标，可同时编辑多行。
⌘+Alt+↓ 向下添加多行光标，可同时编辑多行。

# 编辑类
⌘+J 合并选中的多行代码为一行。举个栗子：将多行格式的CSS属性合并为一行。
⌘+Shift+D 复制光标所在整行，插入到下一行。
Tab 向右缩进。
Shift+Tab 向左缩进。
⌘+K+K 从光标处开始删除代码至行尾。
⌘+Shift+K 删除整行。
⌘+/ 注释单行。
⌘+Shift+/ 注释多行。
⌘+K+U 转换大写。
⌘+K+L 转换小写。
⌘+Z 撤销。
⌘+Y 恢复撤销。
⌘+U 软撤销，感觉和 Gtrl+Z 一样。
⌘+F2 设置书签
⌘+T 左右字母互换。
F6 单词检测拼写

# 搜索类
⌘+F 打开底部搜索框，查找关键字。
⌘+shift+F 在文件夹内查找，与普通编辑器不同的地方是sublime允许添加多个文件夹进行查找，略高端，未研究。
⌘+P 打开搜索框。举个栗子：1、输入当前项目中的文件名，快速搜索文件，2、输入@和关键字，查找文件中函数名，3、输入：和数字，跳转到文件中该行代码，4、输入#和关键字，查找变量名。
⌘+G 打开搜索框，自动带：，输入数字跳转到该行代码。举个栗子：在页面代码比较长的文件中快速定位。
⌘+R 打开搜索框，自动带@，输入关键字，查找文件中的函数名。举个栗子：在函数较多的页面快速查找某个函数。
⌘+： 打开搜索框，自动带#，输入关键字，查找文件中的变量名、属性名等。
⌘+Shift+P 打开命令框。场景栗子：打开命名框，输入关键字，调用sublime text或插件的功能，例如使用package安装插件。
Esc 退出光标多行选择，退出搜索框，命令框等。

# 显示类-屏幕
⌘+Tab 按文件浏览过的顺序，切换当前窗口的标签页。
⌘+PageDown 向左切换当前窗口的标签页。
⌘+PageUp 向右切换当前窗口的标签页。
Alt+Shift+1 窗口分屏，恢复默认1屏（非小键盘的数字）
Alt+Shift+2 左右分屏-2列
Alt+Shift+3 左右分屏-3列
Alt+Shift+4 左右分屏-4列
Alt+Shift+5 等分4屏
Alt+Shift+8 垂直分屏-2屏
Alt+Shift+9 垂直分屏-3屏
⌘+K+B 开启/关闭侧边栏。
F11 全屏模式
Shift+F11 免打扰模式

`update`
⌘+k+2 折叠注释和方法
⌘+k+3 折叠if
⌘+k+4 折叠switch


[Sublime Text 全程指南](http://zh.lucida.me/blog/sublime-text-complete-guide/)



