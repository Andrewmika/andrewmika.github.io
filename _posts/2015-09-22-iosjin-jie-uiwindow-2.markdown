---
layout: post
title: "《iOS进阶》-UIWindow"
date: '2015-09-22 17:20:20'
tags:
- ios
- iosjin-jie
- xue-xi-bi-ji
---

### UIWindow
UIWindow是最顶层的界面容器，继承自UIView。作用如下：

- 作为UIView的最顶层容器，包含应用显示所需要的所有UIView。
- 传递触摸消息和键盘事件给UIView。

#### 为UIWindow增加UIView

- 通过`addSubView`方法。
- 通过特有的`rootViewController`属性。通过设置该属性为要添加view对应的UIViewController,UIWindow会自动将其View添加到当前window中，同时负责维护ViewController和view的生命周期。

#### 系统对UIWindow的使用
通常在一个程序中只会有一个UIWindow，但是有时候调用系统控件时（如UIAlertView），iOS系统为了保证控件在所有的界面之上，他会临时创建一个新的UIWindow,通过将其UIWindow的UIWindowLevel设置得更高，让控件盖在所有的应用界面之上。

UIWindow的`UIWindowLevel`属性定义了UIWindow的层级。一共有三种取值,默认为0：

```
typedef CGFloat UIWindowLevel;
UIKIT_EXTERN const UIWindowLevel UIWindowLevelNormal;
UIKIT_EXTERN const UIWindowLevel UIWindowLevelAlert;
UIKIT_EXTERN const UIWindowLevel UIWindowLevelStatusBar;

```

#### 手动创建UIWindow
当我们想将某些界面覆盖在所有界面的最上层时，我们可以手动创建一个UIWindow.UIWindow一旦被创建，他就自动被添加到整个界面之上了。对于复杂的界面可以继承UIWindow，在子类中写相关逻辑，可以用单例模式创建等等，根据不同情况来设置。

如果我们创建的UIWindow需要处理键盘事件，那就需要合理的将其设置为KeyWindow。keyWindow是被系统设计用来接收键盘和其他非触摸事件的UIWindow.我们可以通过`makeKeyWindow`和`resignKeyWindow`方法来将自己创建的UIWindow实例设置成keyWindow.

适合用UIWindow来实现的功能有：密码输入界面、应用启动介绍页、应用内的通知提醒显示和应用内的弹框广告等。

#### 不要滥用UIWindow
如果弹出界面明显属于某个ViewController,那么更适合把弹出的界面当做这个ViewController的view的subView实现。

常见的滥用方式是把需要弹出的界面都设置成单例，需要的时候就调用显示，这种做法会使新创建的UIWindow一直得不到释放。当出现多个UIWindow需要相互有层级覆盖关系时，实现起来会比较复杂。