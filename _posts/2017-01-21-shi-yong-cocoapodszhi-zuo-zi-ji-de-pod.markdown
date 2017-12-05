---
layout: post
title: 使用Cocoapods制作自己的pod
date: '2017-01-21 10:44:33'
tags:
- cocoapods
- si-you-ku
---

## 前言
现在大部分的开发者都是用Cocopods来管理第三方库，Cocoapods为库的管理提供了便利。在项目的发展当中会出现很多公共组件，如果只是单纯拷贝代码，维护起来会变得非常麻烦，因此制作私有库将给组件管理带来极大遍历，而Cocoapods也提供了这样的功能。

文章分为两部分，第一部分是私有库的制作，第二部分介绍开源自己的库，将自己的库发布到Cocoapods公共仓库。

## 新建仓库
Cocoapods使用podspec来管理库的信息，因此需要一个仓库来管理这些podspec文件。如果要创建私有库就需要一个一个自己的仓库来存储podspec文件，如果开源就可以上传到Cocoapods公有的仓库。因此，创建私有库之前我们先创建私有的spec repo。

### 创建私有Spec Repo

1. 创建spec的git仓库
    `http://192.168.1.100:3000/AndrewShen/MySpecRepo.git`
    
2. 添加创建的`Spec Repo`

  使用命令行添加仓库到本地repo,会自动建立git的链接
  
```ruby
  # pod repo add REPO_NAME SOURCE_URL
  $ pod repo add MySpecRepo http://192.168.1.100:3000/AndrewShen/MySpecRepo.git
```
  
  将会执行:
  
```Ruby
  Cloning spec repo `MySpecRepo` from `http://192.168.1.100:3000/AndrewShen/MySpecRepo.git`
```
  
  执行成功后可以执行`pod repo`查看已有的repo
  

## 新建pod

### 从零开始创建pod

  1. 如果需要从头开始写项目，开发组件，那可以通过`pod lib create`来创建工程
    我们执行
```Ruby
    #pod lib create LIBNAME
    pod lib create podDemo
```
    之后会问你四个问题，问完后自动执行`pod install`并生成依赖：
      1. 使用什么语言
      2. 是否需要一个例子工程
      3. 选择一个测试框架
      4. 是否基于View测试
      5. 类的前缀
    如果执行`pod install`失败，则删掉PodFile里的test target就行了。

  2. 创建成功后我们可以看到目录结构，我们的pod会存在于Example项目的Pods的`Development Pods`目录下。
  
```
    podDemo
    ├── Example
    │   ├── Podfile
    │   ├── Podfile.lock
    │   ├── Pods
    │   ├── Tests
    │   ├── podDemo
    │   ├── podDemo.xcodeproj
    │   └── podDemo.xcworkspace
    ├── LICENSE
    ├── README.md
    ├── _Pods.xcodeproj -> Example/Pods/Pods.xcodeproj
    ├── podDemo
    │   ├── Assets
    │   └── Classes
    └── podDemo.podspec
```
  
  3.向pod文件夹（此处为podDemo）中加入资源文件（`Assets`中）和库文件（`Classes`中），

  4. 在example文件夹下执行`pod update`更新工程pod。成功后`Development Pods`目录下我们的pod文件便会更新。**只要更新了pod文件夹下内容或者更改了podspec文件都需要执行`pod update`**
  
  5. 通过Cocoapods创建的项目会在本地的git管理下，我们需要将其添加到远程仓库。
  
```ruby
    git add .
    git commit -m "Initial"
    git remote add origin http://git.celebi.com/AndrewShen/podDemo.git           #添加远端仓库
    git push origin master     #提交到远端仓库
```
    
  6. 由于`podspec`文件pod源需要`tag`号，因此我们需要为项目打上`tag`.
  
```ruby
   git tag -a 0.0.1 -m "tag release 0.0.1" #打tag
   git push --tags #提交tag
```
    
  7. 编辑`podspec文件`
  
```Ruby
    Pod::Spec.new do |s|
      s.name             = 'podDemo' #pod名称
      s.version          = '0.1.0'   #pod版本
      s.summary          = '测试pod简介.' #简介，需要更改，不然会报警告
    
    # This description is used to generate tags and improve search results.
    #   * Think: What does it do? Why did you write it? What is the focus?
    #   * Try to keep it short, snappy and to the point.
    #   * Write the description between the DESC delimiters below.
    #   * Finally, don't worry about the indent, CocoaPods strips it!
    
      s.description      = <<-DESC      #详细介绍，要比简介长
                            测试pod的详细介绍
                           DESC
    
      s.homepage         = 'http://192.168.1.100:3000/AndrewShen/podDemo' # 项目主页
      # s.screenshots     = 'www.example.com/screenshots_1', 'www.example.com/screenshots_2'
      s.license          = { :type => 'MIT', :file => 'LICENSE' }  #协议
      s.author           = { 'AndrewShen' => 'andrewshen@minture.com' }  # 开发者信息
      s.source           = { :git => 'http://git.celebi.com/AndrewShen/podDemo.git', :tag => s.version.to_s } #仓库地址
      # s.social_media_url = 'https://twitter.com/<TWITTER_USERNAME>'
    
      s.ios.deployment_target = '7.0' # 最低版本
    
      s.source_files = 'podDemo/Classes/**/*' # 库文件
      
      # s.resource_bundles = {                     #资源目录
      #   'podDemo' => ['podDemo/Assets/*.png']
      # }
    
      # s.public_header_files = 'Pod/Classes/**/*.h'
      s.frameworks = 'UIKit', 'MapKit'   #依赖的framework
      # s.dependency 'AFNetworking', '~> 2.3'  # 依赖的第三方库  
```
    
  8. 编辑后使用`pod lib lint`验证`podspec文件是否可用`。如果有`Error`和`Warning`是无法添加到`spec repo`中的。但是`Warning`可以存在，可以使用选项`--allow-warnings`忽略警告。
  
```
    pod lib lint
```
    
  如果显示为以下则说明创建成功
  
```Ruby
    -> podDemo (0.1.0)
    podDemo passed validation.
```
    
### 从组件项目中创建pod

  如果已经有组件项目/Demo则只需要创建podspec文件。
  1. 执行命令创建podspec文件：
  
```Ruby
  pod spec create ADSSegmentedButton https://github.com/Andrewmika/ADSSegmentedButton.git
```
  
  2. 创建完成后打开我们创建的podspec文件，进行编辑。
  
  3. 验证文件是否可用
  
    执行命令`pod lib lint`
    
    如果显示为以下则说明创建成功
    
```Ruby
    -> ADSSegmentedButton (0.0.1)

    ADSSegmentedButton passed validation.
```

## pod本地测试

配置好`podspec`文件后可以在本地测试pod是否可用，然后发布到服务器上.
在`Podfile`中指定`pod`，可以指定_路径_或者_文件_

```
platform :ios, '7.0'

pod 'PODNAME', :path => '项目路径'      # 指定路径
pod 'PODNAME', :podspec => 'podspec文件路径'  # 指定podspec文件
```

由于还未将`pod`提交到`spec repo`中，因此`pod install`后的库文件仍然在`Development Pods`目录下

## pod私有库发布

`pod`发布只需要在podspec文件下执行一行命令，此命令会自动将文件push到git仓库中：

```
#pod repo push REPONAME [NAME.podspec]
pod repo push MySpecRepo podDemo.podspec
```

如果想要看详情可以加上选项`--verbose`。一般报错就是`tag`没打好，提取不到tag代码，无法验证podspec也就无法push。

## 发布到CocoaPods

### 注册帐号

```Ruby
#pod trunk register YOUREMAIL 'NAME' --description='DESCRIPTION'
pod trunk register orta@cocoapods.org 'Orta Therox' --description='macbook air'
```

将用户名，邮箱换成自己的，给设备添加描述信息。由于CocoaPods没有用户名密码，使用的是验证令牌，所以换一台电脑的话还需要执行以上命令。
执行成功后会提示以下信息,点击连接完成注册。

```
[!] Please verify the session by clicking the link in the verification email that has been sent to 『your email』
```

完成后可以执行命令`pod trunk me`查看自己账户信息。

### 发布pod

发布到公有库也只需要一行命令：

```Ruby
pod trunk push [NAME.podspec]
```

之后也会验证podspec,通过后上传。提交到公有库速度比较慢，需要耐心等待。