---
layout: post
title: "《iOS进阶》-iOS开发工具使用"
date: '2015-09-14 06:05:11'
tags:
- ios
- iosjin-jie
- xue-xi-bi-ji
---

### 1. 使用CocoaPod做依赖管理
CocoaPods是iOS的一种依赖管理工具，[项目源码](https://github.com/CocoaPods/CocoaPods)托管在GitHub上。
我之前整理过一篇关于CocoaPods的文章，详见[iOS依赖管理工具的使用-CocoaPods](http://iandrew.space/iosyi-lai-guan-li-gong-ju-de-shi-yong-cocoapods-carthage/)。

### 2. 网络封包分析工具Charles
Charles是Mac下常用的截取网络封包的工具，使用它截取客户端与服务器通信的接口信息非常方便，同时还能模拟网络状态，是iOS开发不可或缺的一个优秀的开发工具。

#### 2.1 安装与配置
安装好Charles需要进行配置，首先安装`root certificate`,点击`Help`->`SSL Proxying`->`Install Charles Root Certificate`。然后将Charles设为系统代理：点击`Proxy`,勾选`Mac OS X Proxy`,这样，Charles便能截取到网络请求。

Charles主要提供两种封包视图，分别为`Structure`和`Sequence`，他们的功能分别为：

1. Structure将网络请求按访问的域名分类。
2. Sequence将网络请求按照访问的时间排序。

#### 2.2 网络封包过滤
通常我们只需要监测我们需要的网络地址，所以我们要从众多网络请求中过滤出我们需要的请求。设置过滤有两种方法：

1. Sequence界面，在`Filter`栏输入要过滤出来的关键字。这种方式做一些临时性的封包过滤。
2. 菜单栏选择`Proxy`->`Recording Settings`,选择`include`栏，点击`add`填入需要监控的协议，主机地址，端口号。这种方式作为我们经常性的封包过滤。

#### 2.3 截取iPhone设备上的网络封包。
要截取iPhone上的网络封包，我们首先需要将Charles的代理功能打开。在菜单栏选择`Proxy`->`Proxy settings`，填入代理端口`8888`，并且勾选`Enable transparent HTTP proxying`。

之后在iPhone的Wifi中打开当前连接的wifi详情，选择底部`HTTP 代理`一项，将代理模式切换为手动，并填入，Charles运行所在电脑的IP地址，并将端口号设为8888。设置完毕。当我们打开iPhone上的需要网络通讯的程序，Charles便会弹出连接请求，选择allow，即可截取iPhone的网络封包。

#### 2.4 模拟慢速请求
在菜单栏上选择`Proxy`->`Throttle Setting`,在弹出的对话框中设置Throttle Preset的类型，并勾选`Enable Throttling`,即可模拟网络状态。如果我们只想模拟指定的慢速网络，可以再勾选`Only forselected hosts`项，然后在对话框的下半部分设置中增加指定的Hosts项即可。

#### 2.5 截取SSL信息
Charles 默认并不截取SSL信息，如果想要截取摸个网站上的SSl网络请求，可以在改请求上单击右键，选择`SSL Proxying`。

#### 2.6 修改网络请求的内容
iOS开发中我们需要不断调试网络接口，尝试不同的网络参数，Charles提供了网络请求修改和重发功能。使用修改功能只需要右键单击网络请求，选择`Edit`,即可创建一个可编辑的网络请求。我们可以修改请求的任何信息，包括URL地址，端口，参数等，之后点击`Execute`按钮发送修改后的网络请求。

#### 2.7 修改服务器返回的内容
我们可以修改服务器返回的内容，方便我们的调试。根据具体需求，Charles提供了Map功能，Rewrite功能和BreakPoints功能，他们都能达到修改服务器返回内容的目的。

1. Map功能适合长期的将某一些请求重定向到另一个网络地址或本地文件。
2. Rewrite功能适合对网络请求进行一些正则替换。
3. BreakPoints功能适合做一些临时性的修改。 

#### 2.8 Map功能
map功能分为`Map Remote`和`Map Local`两种，`Map Remote`是将指定网络请求重定向到另一个网址，`Map Local`是将指定的网络请求重定向到本地文件。

设置方法：`Tools`->`Map Remote`或`Map Local`即可进入功能设置页面。对于一些复杂的网络请求结果，我们可以先使用Charles提供的`save response`功能 ，将请求的结果保存到本地，稍加修改使其成为我们的目标映射文件。

#### 2.9 Rewrite功能
Rewrite功能适合对某一类网络请求进行一些正则替换，以达到修改结果的目的。适合批量和长期的替换。

#### 2.10 Breakpoints功能
Breakpoints功能适合临时性的修改。它类似于Xcode中设置断点，当指定的网络请求发生时，Charles会截取该请求，这时我们可以在Charles中临时修改网络请求的返回内容。修改完后单击`Execute`按钮就可以让网络请求继续进行。

### 3. 界面调试工具Reveal
Reveal类似于Xcode中的界面调试工具，但是功能更强大，还可以查看别的程序的内容。

###4. 移动统计工具Flurry
`Flurry`是一家专门为移动应用提供数据统计和分析的公司，类似的还有`Google Analytics`，国内的`友盟`

### 5. 崩溃日志记录工具Crashlytics
`Crashlytics`是专门为移动应用开发者提供的保存和分析应用崩溃信息的专业工具。现在Apple也出了自家的崩溃日志记录工具。优点如下：

1. 不会漏掉任何应用崩溃信息。
2. 可以像Bug管理工具那样，管理崩溃日志。
3. 可以每天和每周将崩溃信息汇总发到你的邮箱。

### 6. App Store统计工具App Annie
`App Annie`是一个App Store数据的统计分析工具。该工具可以统计App在App Store的下载量、排名变化、销售收入情况及用户评价等信息。

### 7. Xcode插件
目前很多大牛都开发了很多优秀的插件，但是存在一个问题就是更新Xcode之后这些插件一般都需要更新，如果作者不更新插件可能就无法使用。

#### 7.1 插件管理工具Alcatraz
`Alcatraz`是一个能帮你管理Xcode插件，模板及颜色配置的工具。他可以直接集成到Xcode的图形界面中。安装好之后可以在`Window`->`Package Manager`中打开，使用简单。

#### 7.2 常用插件
- KSImageNamed:帮助输入图片资源名的插件。
- XVim:可以开启Vim模式，全键盘操作。
- FuzzyAutocompletePlugin:模糊代码补全。
- XToDo:查找项目中所有的带有TODO、FIXME、？？？、！！！标记的注释。
- BBUDebuggerTuckAway:可以在编辑代码的时候自动隐藏地步调试窗口。
- SCXcodeSwitchExpander:能帮你迅速在switch语句中填充枚举类型的每种可能的取值。
- deriveddata-exterminator:是一个清除Xcode缓存目录的插件，当Xcode显示一些编译的错误或警告，但是最终却编译通过，这时就需要清除一下Xcode缓存。
- VVDocumenter:是一个自动生成代码注释的工具。
- ClangFormat:是一个自动调整代码风格的工具。
- ColorSense:是一个UIColor颜色输入辅助工具，实时预览颜色。
- XcodeBoost:包含多个辅助修改代码的小功能。如

	将.m文件中方法的定义暴露到对应的.h文件中。
	在某一个源文件中直接输入正则表达式查找。
	可以复制粘贴代码时，不启用Xcode的自动缩进功能。
- CocoaControls:
- HOStringSense:字符串输入插件，帮助将特殊字符转义为String。
- XLGotoSandbox:快速进入应用程序沙盒。
- ZLXCodeLine:统计代码行数。

#### 7.4 插件失效问题的解决
Xcode插件存放在`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins`目录下，为.xcpluging格式.通过里面的显示包内容会有一个`Info.plist`文件，打开文件，可以看到一个字段叫`DVTPlugInCompatibilityUUIDs`,这个字段存储能使用次插件的Xcode的UUID。如果当前版本的插件失效，那就将当前Xcode版本的UUID加入到`DVTPlugInCompatibilityUUIDs`中。

查看Xcodee的信息可以通过打开Xcode的包内容中的plist文件查看`DVTPlugInCompatibilityUUIDs`。我们可以通过命令``` find ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins -name Info.plist -maxdepth 3 | xargs -I{} defaults write {} DVTPlugInCompatibilityUUIDs -array-add `defaults read /Applications/Xcode.app/Contents/Info.plist DVTPlugInCompatibilityUUID` ```批量向插件增加UUID。

有人也写了一个小脚本：[cikelengfeng/RPAXU](https://github.com/cikelengfeng/RPAXU)，一样的效果。

#### 7.5 手贱点击Skip Bundle解决办法
在命令行中输入`defaults delete com.apple.dt.Xcode DVTPlugInManagerNonApplePlugIns-Xcode-7.1`，最后数字为Xcode版本号，重启Xcode便会提示加载bundle了。

### 8. 其他工具介绍

#### 8.1 取色工具：自带数码测色计（DigitalColor Meter）

#### 8.2 取色，测量工具：xScope

#### 8.3 图像压缩工具：ImageOptim

#### 8.4 标注工具：马克鳗（MarkMan）

#### 8.5 API文档查询及代码片段管理工具：（Dash）

#### 8.6 命令行工具

- nomad:方便你操作苹果开发者中心。
- xctool：Facebook开源的一个iOS编译和测试的工具。
- appledoc:从开源代码中抽取文档的工具。 