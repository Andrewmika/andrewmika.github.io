---
layout: post
title: "《iOS进阶》-开发技巧"
date: '2015-09-23 16:25:01'
tags:
- ios
- iosjin-jie
- xue-xi-bi-ji
---

### 收起键盘
在UIViewController中收起键盘，除了调用相应控件的resignFirstResponder方法外，还有另外三种方法。

- 重载touchesBegin方法，在里面执行`[self.view endEditing:YES]`;这样单击UIViewController的任意地方就可以收起键盘。
- 在获得当前UIViewController比较困难时，可以直接执行`    [[UIApplication sharedApplication]  sendAction:@selector(resignFirstResponder) to:nil from:nil forEvent:nil];`。
- 直接执行`[[[UIApplication sharedApplication] keyWindow] endEditing:YES];`。

### 设置应用内的系统控件语言
在UIWebView,相册，UITableViewCell删除状态等控件不会根据手机当前系统语言来进行设置，二十根据应用内部语言来设置的。

设置方法，直接在工程 Info.plist文件中增加CFBundleLocalizations键的值，zh_CN,en，等。

### 截屏
iOS的截屏功能可以将当前界面中的UI元素保存成UIImage.在iOS7之后，可以通过API,`- (UIView *)snapshotViewAfterScreenUpdates:(BOOL)afterUpdates NS_AVAILABLE_IOS(7_0);
`来实现。我们根据不同需要进行截图的操作，如半透明效果等。

### 关于内存优化
iOS6以后不建议将view置为nil，原因如下：
- UIView有一个CALayer的成员变量，CALayer是具体用于将自己画到屏幕上的。
- CALayer是一个bitmap图像的容器，当UIView调用自身的drawRect时，CALayer才会创建这个bitmap图像类。
- 具体占内存是一个bitmap图像类，CALayer只占48Bytes,UIView只占96Bytes.
- 当系统发出MemoryWarning时，系统会自动回收bitmap类，但是不回收UIView和CALayer类。这样既能回收大部分内存，又能在需要bitmap类时，通过调用UIView的drawRect：方法重载。

### Xcode使用技巧
#### Xcode快捷键

- Cmd + shift + O 快速查找类，通过这个可以快速跳到指定类的源代码中。
- Ctrl + 6 列出当前文件中的所有方法，可以输入关键字过滤。
- Cmd + Ctrl + Up 在.h 和 .m之间切换
- Cmd + Shift + Y切换Console Vie的显示隐藏
- Cmd + Ctrl + Left/Right 到上/下一次编辑的位置
- Cmd + Opt + J 跳转到文件过滤区
- Cmd + Shift + F 在工程中查找
- Cmd + R 运行
- Cmd + B 编译工程
- Cmd + Shift + K 清空编译好的文件
- Cmd + . 结束本次调试
- Esc 调出代码补全
- Cmd + 单击 查看方法实现
- Opt + 单击 查看方法文档
- Cmd + T 新建Tab栏
- Cmd + Shift + [ 在Tab栏之间切换

#### 查找技巧
Xcode的查找功能提供了Insert Pattern的方式，方便你输入常见的查找规则。

#### JS文件调整
JS文件不需要编译，如果JS文件出现在Compile Source中需要将其移到Copy Bundle Resources中

#### 清除DerivedData
当多次重构工程造成代码没有错误却编译失败时，可以尝试删除DerivedData目录。DerivedData目录是Xcode的编译缓存。

#### target信息异常
当工程的编译target信息异常时，可以删除XXXX.xcodeproj/xcuserdata目录。该目录下存有当前用户的各种工程状态信息，删除后重启Xcode会自动重建。

#### 代码片段（Code Snippets）
当你觉得某段代码很有用，可以当做模板的时候，可以将其选中，拖到Xcode的代码片段区即可，作为自定义的代码片段。

当在不同的电脑上工作时，可以用git来管理代码片段，方便在不同设备上同步开发工具的设置。

