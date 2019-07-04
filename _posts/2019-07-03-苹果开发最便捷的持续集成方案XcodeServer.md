---
layout: post
title: "苹果开发最便捷的持续集成方案—Xcode Server"
date: '2019-07-04 18:00:33'
comments: true
---

如果要进行iOS持续集成，最流行的大概是Jenkins，Fastlane。但是目前来看这些配置都比较复杂，而最简单的是使用`Xcode Server`，但是网上资料却比较少。

`Xcode Server`由苹果开发，与Xcode高度集成，可对代码进行静态分析、单元测试、打包等。对苹果开发来说是最友好的一种方式。

`Xcode 9`，是`Xcode Server`的一次转折。在此之前需要安装MacOS Server 在其基础上进行配置，虽然能够实现CI,但Bug比较多。

从`Xcode 9`开始，`Xcode Server`就集成到了Xcode中，只需要安装Xcode就能进行CI操作，而且配置操作变得极其简单，证书也可以自动配置。

## 步骤一 -- 开启Xcode Server
在你希望用于持续集成的Mac电脑上开启Xcode Server

打开Xcode -> Preferences -> 选中Server & Bots -> **Turn On**

![](https://raw.githubusercontent.com/Andrewmika/MyPicBed/master/img/42A1C222656AD8731057580E0719CB49.jpg)

## 步骤二 -- 连接Xcode Server
在你开发的电脑上添加Xcode Server服务器。

打开Xcode -> Preferences -> Accounts -> 添加Xcode Server -> 直接选择对应电脑或者通过IP连接

![](https://raw.githubusercontent.com/Andrewmika/MyPicBed/master/img/9DF8054149615642CAE0207D8D12054C.jpg)

![](https://raw.githubusercontent.com/Andrewmika/MyPicBed/master/img/EADA133689C37A0677C370AE3916933A.jpg)

添加成功后将会在Report Navigator中看到添加的服务器。
![](https://raw.githubusercontent.com/Andrewmika/MyPicBed/master/img/355A2C8486C225153A76D6AA0A4F4E3A.jpg)

## 步骤三 -- 创建bot
`Xcode Server`中CI任务由`Bot`进行管理
创建由两种方式，一种通过Product -> Create Bot 创建。另一种在`Report Navigator`中选中添加的服务器，右键Create Bot.

取名->配置仓库->配置Scheme,静态分析，测试，打包等->集成的频率->证书配置->设备->xcodebuild参数和环境变量->打包触发器

![](https://raw.githubusercontent.com/Andrewmika/MyPicBed/master/img/26A9025C8E11665870518A7587829FAA.jpg)
![](https://raw.githubusercontent.com/Andrewmika/MyPicBed/master/img/F507806D87CBEE453BC90A6D211A08A1.jpg)


如果我们担心打出的包是Adhoc版本还是Appstore版本，我们可以通`ExportOptions.plist`来进行配置。如下图：
![](https://raw.githubusercontent.com/Andrewmika/MyPicBed/master/img/3449F6F859DC38BF1198030F43FA9593.jpg)

Trigger，这部分可以添加集成前、集成后需要触发的脚本，还有右键通知配置。

一般开发中会使用cocoapods,也需要将包上传至蒲公英和appStore.
这里提供几个脚本

### pod install

```sh
#!/bin/sh
export LANG=en_US.UTF-8

cd ${XCS_PRIMARY_REPO_DIR}

pwd

rm -f Podfile.lock

/usr/local/bin/pod install
```

这里需要注意的时pod 的路径。直接执行`pod install`可能会提示无法找到`pod`命令。因此需要直接载入完整路径。

在terminal中执行`where pod`，找到打包机上的pod路径
```
$ where pod
/usr/local/bin/pod
```

### 上传蒲公英

```sh
#!/bin/sh

#请根据蒲公英自己的账号，将其中的 uKey 和 _api_key 的值替换为相应的值。

curl -F "file=@${XCS_PRODUCT}" -F "uKey=你的ukey" -F "_api_key=你的apiKey" -F "updateDescription=${MSG}" -F "password=${PASSWORD}" http://www.pgyer.com/apiv1/app/upload
```

### 上传AppStore
```sh
#!/bin/sh
altoolPath="/Applications/Xcode.app/Contents/Applications/Application Loader.app/Contents/Frameworks/ITunesSoftwareService.framework/Versions/A/Support/altool"
# 填入你的Apple ID
USERNAME="你的AppleID"
#  需要去Apple ID账户生成 App 专用密码
PASSWORD="App 专用密码"

"$altoolPath" --validate-app -f ${XCS_PRODUCT} -u ${USERNAME} -p ${PASSWORD} -t ios
"$altoolPath" --upload-app -f ${XCS_PRODUCT} -u ${USERNAME} -p ${PASSWORD} -t ios

```

## 更详细的文档
请参阅以前翻译的部分，一些配置还是可以用的[Xcode Server中文翻译](https://github.com/Andrewmika/Xcode-Server-and-Continuous-Integration-Guide-in-Chinese)