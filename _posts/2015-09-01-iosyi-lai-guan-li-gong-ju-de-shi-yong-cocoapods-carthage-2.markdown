---
layout: post
title: iOS依赖管理工具的使用-CocoaPods
date: '2015-09-01 09:53:27'
---

文章参考自[《用CocoaPods做iOS程序的依赖管理》](http://blog.devtang.com/blog/2014/05/25/use-cocoapod-to-manage-ios-lib-dependency/)，在此基础上进行了更改和增加。

### 依赖管理工具简介
在项目开发过程当中，总会用到一些优秀的开源库。如果按照比较传统的方法便是在GitHub上面下载好源代码，再拖到工程当中。如果遇到第三方框架需要其他依赖，还要设置`-fno-objc-arc`等编译参数的时候，那就让人头大了。这时候依赖管理工具的作用就体现出来了。目前iOS的依赖管理工具有**CocoaPods**和[**Carthage**](https://github.com/Carthage/Carthage)(只支持iOS8+)。

### CocoaPods的安装和使用
#### 安装CocoaPods
Mac自带Ruby，因此使用如下命令即可安装：

```
$ sudo gem install cocoapods
$ pod setup
```
所有的项目的podspec文件都托管在`https://github.com/CocoaPods/Specs`,在执行`pod setup`时，CocoaPods会将这些podspec索引文件更新到本地`~/.cocoapods/`目录下，非常慢。这时可以更换CocoaPods的镜像一个叫[akinliu](http://akinliu.github.io/2014/05/03/cocoapods-specs-/)的朋友在[gitcafe](https://gitcafe.com/)和[oschina](http://www.oschina.net/)上建立了CocoaPods索引库的镜像.分别为`https://gitcafe.com/akuandev/Specs.git`和`http://git.oschina.net/akuandev/Specs.git`.使用如下命令进行更换

```
$ pod repo remove master
$ pod repo add master https://gitcafe.com/akuandev/Specs.git
$ pod repo update
```

如果自带的Ruby软件源rubygems.org使用了亚马逊的云服务，所以被墙了，可以替换成淘宝的源。

```
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
$ gem sources -l
```

#### 使用CocoaPods
使用第三方库，我们需要生成一个podfile文件，里面将包含我们将要使用的第三方库的名字和版本。

-  当我们新建project之后，使用命令行cd到工程目录，使用`$ pod init`即可在当前文件夹中生成podfile文件。
-  查找第三方库：通过`pod search 库名` 

```
		$ pod search SDWebImag
		-> SDWebImage (3.7.3)
	  	Asynchronous image downloader with cache support with an UIImageView
	  	category.
  	 	pod 'SDWebImage', '~> 3.7.3'
  	 	- Homepage: https://github.com/rs/SDWebImage
  	 	- Source:   https://github.com/rs/SDWebImage.git
  	 	- Versions: 3.7.3, 3.7.2, 3.7.1, 3.7.0, 3.6, 3.5.4, 3.5.2, 3.5.1, 3.5, 3.4,	 	  3.3, 3.2, 3.1, 3.0, 2.7.4, 2.7, 2.6, 2.5, 2.4 [master repo] - 3.7.3, 3.7.2,	 	  3.7.1, 3.7.0, 3.6, 3.5.4, 3.5.2, 3.5.1, 3.5, 3.4, 3.3, 3.2, 3.1, 3.0, 2.7.4
		   2.7, 2.6, 2.5, 2.4 [master-1 repo]
  	 	- Subspecs:
	     - SDWebImage/Core (3.7.3)
	     - SDWebImage/MapKit (3.7.3)
	     - SDWebImage/WebP (3.7.3
```
- 复制`pod 'SDWebImage', '~> 3.7.3'`将其放到podfile中可以使用vim命令,`vim podfile`打开文件，按`i`键进入编辑模式，将`pod 'SDWebImage', '~> 3.7.3'`粘贴到文件中，点击`esc`退出编辑模式，按住`shift`+按两次`z`保存并退出

```
	# Uncomment this line to define a global platform for your project
	# platform :ios, '6.0'

	target 'UIKitDemo' do
	`pod 'SDWebImage', '~> 3.7.3'`
	end
	
	target 'UIKitDemoTests' do
	
	end
	
	~                                                                               
	~                                                                               

```

- 然后执行`pod install --no-repo-update`进行安装,之前使用的`pod install`，不过好像被墙了，不能用，加上`--no-repo-update`禁止做其他索引更新操作，速度会快很多。cocoaPods会将podfile里的第三方库导入到你的project中，并自动配置。完成之后需要使用`.xcworkspace`文件来打开工程，不能使用`.xcodeproj`文件。
- 当新加第三方库到podfile文件中时，可以使用`pod install --no-repo-update`或`pod update --no-repo-update`,后者会更新第三方库的版本。

#### 关于.gitignore
当你执行`pod install`之后，除了Podfile外，CocoaPods还会生成一个名为`Podfile.lock`的文件，你不应该把这个文件加入到`.gitignore`中。因为`Podfile.lock`会锁定当前各依赖库的版本，之后如果多次执行`pod install` 不会更改版本，要`pod update`才会改`Podfile.lock`了。这样多人协作的时候，可以防止第三方库升级时造成大家各自的第三方库版本不一致。

CocoaPods的这篇官方文档也在`What is a Podfile.lock`一节中介绍了`Podfile.lock`的作用，并且指出：

**This file should always be kept under version control.**


	
