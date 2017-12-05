---
layout: post
title: "《iOS进阶》-GCD使用"
date: '2015-09-22 17:16:08'
tags:
- ios
- iosjin-jie
- xue-xi-bi-ji
---

### GCD的使用

#### block的定义
block 有点像函数指针，只不过用**"^"**代替了**"*"**

- 申明变量：`(void)(^loggerBlock)(void);`
- 定义：`loggerBlock = ^{ NSLog(@"Hello World");};`
- 调用：`loggerBlock();`

block特点：

1. 程序块可以在代码中以内联的方式来定义。
2. 程序块可以访问在创建它的范围内的可用变量。

#### 系统的dispatch方法
苹果提供了一些方法方便我们将block放在主线程或后台线程执行或咽喉执行。

- 后台执行：

```
     dispatch_async(dispatch_get_global_queue(0, 0), ^{
        // something
    });
```
- 主线程执行：

```
dispatch_async(dispatch_get_main_queue(), ^{
        // something
    });
```
- 一次执行：

```
   static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        // code to be executed once
    });
```
- 延迟执行：

```
double delayInSeconds = 2.0;
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(delayInSeconds * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        // code to be executed after a specified delay
    });
```

- 自定义dispatch_queue_t,使用dispatch_queue_create方法:

```
 dispatch_queue_t queue = dispatch_queue_create("hello", NULL);
    dispatch_async(queue, ^{
        // something
    });
```
- 多线程并行执行，等线程执行结束在汇总执行结果。使用`dispatch_group`,`dispatch_group_async`和`dispatch_group_notify`来实现：

```
    dispatch_group_t group = dispatch_group_create();
    dispatch_group_async(group, dispatch_get_global_queue(0, 0), ^{
        // 并行线程一
    });
    dispatch_group_async(group, dispatch_get_global_queue(0, 0), ^{
        // 并行线程二
    });
    dispatch_group_notify(group, dispatch_get_global_queue(0, 0), ^{
        // 汇总结果
    });

```

#### 修改block之外的变量
默认情况下，在block中访问的外部变量是复制过去的，即写操作不对原变量生效。但是可以加上**__block**来让写操作生效：

```
__block int a = 0;
void (^foo)(void) = ^{
	a = 1;
}
foo(); // a 为 1
```

#### 后台运行
在退出程序回到home页时可以调用UIApplication的`beginBackgroundTaskWithExpirationHandler`方法,让程序最多有十分钟的时间在后台长久运行。这个时间我们可以用来清理本地缓存，发送统计数据等。

```
// AppDelegate.h中
@property (nonatomic,assign) UIBackgroundTaskIdentifier backgroundUpdateTask;  

// AppDelegate.m中
- (void)applicationDidEnterBackground:(UIApplication *)application
{
    [self beginBackgroundUpdateTask];
    
    // 长久运行代码
    
    [self endBackgroundUpdateTask];
}

- (void)beginBackgroundUpdateTask {

self.backgroundUpdateTask = [[UIApplication sharedApplication] beginBackgroundTaskWithExpirationHandler:^{
    [self endBackgroundUpdateTask];
}];
}

- (void)endBackgroundUpdateTask {
    [[UIApplication sharedApplication] endBackgroundTask:self.backgroundUpdateTask];
    self.backgroundUpdateTask = UIBackgroundTaskInvalid;
}

```
