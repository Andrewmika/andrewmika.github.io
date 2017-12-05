---
layout: post
title: 使用Xcode Server 进行持续集成
date: '2017-01-21 10:29:10'
---

#前言

Xcode Server是一个简单的持续集成工具，但只支持git。

## 准备工作

1. 一个git代码仓库
2. 一台mac系统的电脑，安装Mac Server（有开发者帐号可以去网站上免费下载）。

# 配置Xcode Server

1. 打开Server，选择Xcode,选取Xcode选择你的Xcode程序
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1121432-fcc14a5c1448a1ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 将右上角的开关打开，支持Server的简单配置即可完成

# 配置Xcode和证书

## 配置证书和描述文件
  ### 导入证书
  1. 将开发证书和发布证书导出为.p12文件，安装到Server 的系统钥匙串中。
  2. 持续继承的包由/usr/bin/codesign管理，所以要将开发证书和发布证书的访问控制添加codesign.右键证书->显示简介->访问控制->添加。usr文件夹为隐藏文件，可使用`Command+Shift+.`显示隐藏文件。
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1121432-8da94d9065292106.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  3. 至此证书配置完成。

## 导入项目描述文件

  我们需要将ProvisioningProfile 导入到Server服务器中，集成时才能识别到。
  
  Xcode描述文件在`当前用户/Library/MobileDevice/ProvisioningProfiles`
  
  Server服务器描述文件在`/Library/Developer/XcodeServer/ProvisioningProfiles`,但是ProvisioningProfiles文件夹没有访问权限，需要右键显示简介更改权限。
    ![Pro1.png]
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1121432-bdae5690ef5acd4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 创建Bot

1.  打开Xcode->打开工程->Product->Create Bot->填写Bot名称，选择Server服务器
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1121432-ded3e381c08b570e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.  点击Next,系统会自动识别仓库地址，再点击Next配置，选择scheme
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1121432-af81f94ad2cbf18f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.  配置打包时机
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1121432-32ed973ab542ff1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.  继续Next 根据自己的需要进行配置，最后Create

## 打包
  配置好项目的打包配置项，手动打包，如果成功了，持续继承也应该成功。
  
  在Report Navigatior中选择`Integrate`，可以立即进行打包，`EditBot`可以编辑bot。
  
  根据配置好的bot打包时机，会自动集成。
  
# 写在最后
  Xcode Server是非常简单的持续继承工具。配置起来主要有三方面。
  1. 安装Server，配置Server和Xcode关联。
  2. 安装开发证书，导入ProvisionningProfiles配置文件
  3. 创建Bot,进行持续集成。