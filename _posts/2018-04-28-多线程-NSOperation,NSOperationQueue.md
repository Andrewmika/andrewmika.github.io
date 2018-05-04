---
layout: post
title: "多线程-NSOperation,NSOperationQueue"
comments: true
---

# NSOperation,NSOperationQueue
[demo](https://github.com/Andrewmika/StudyDemo)。最快熟悉的方式就是自己码一遍。

## NSOperation
NSOperation是一个抽象类，我们可以直接使用其子类`NSInvocationOperation`和`NSBlockOperation`，或者封装`NSOperation`子类来添加要在线程中执行的操作。默认情况下NSOperation单独使用时执行同步操作。

### NSInvocationOperation
操作在当前线程中执行，不开启新线程。

```
  NSInvocationOperation *op1 = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(p_task1) object:nil];
  [op1 start];
```

### NSBlockOperation
- 如果封装多个操作，会自动开启线程，线程数由系统决定。**封装多个操作时不受串行队列控制**。
创建操作：

```
  NSBlockOperation *op = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"--->blockOp1-Thread:%@",[NSThread currentThread]);
    }];
    [op start];
```
添加操作：通过`- (void)addExecutionBlock:(void (^)(void))block;`为`NSBlockOperation`添加额外的操作。这些操作可以在**不同的线程中**并发执行，线程的切换由**系统决定**。如果添加的操作较多，`blockOperationWithBlock:`中的操作可能会在其他线程中执行。

### 自定义NSOperation子类
通过重写`main`或`start`方法来定义自己的`NSOperation`对象。

// 当前线程执行

```
@implementation CustomOperation

- (void)main {
    if (!self.cancelled) {
        NSLog(@"--->customMainOp-Thread:%@",[NSThread currentThread]);
    }
}

@end
```

### 并发执行
需要重写的方法：
必须：
- `start`：是一个operation的起点。重写时不要调用父类的方法。
- `isExecuting`、`isFinished`:operation状态。当值发生变化时需要生成相应的`KVO`通知，以便外界能够观察状态变化。
- `isAsynchronous`：返回是否并发。

可选：
- `main`：实现改operation相关联的任务。用`main`方法实现任务可使 _设置代码_  _任务代码_ 得到分离，从而使operation的结构更加清晰。

```
@implementation ConcurrentOperation

@synthesize executing = _executing;
@synthesize finished = _finished;

- (BOOL)isExecuting {
    return _executing;
}

- (BOOL)isFinished {
    return _finished;
}

- (BOOL)isAsynchronous {
    return YES;
}

- (void)start {
    if (self.cancelled) {
        [self willChangeValueForKey:@"isFinished"];
        _finished = YES;
        [self willChangeValueForKey:@"isFinished"];
        return;
    }
    [self willChangeValueForKey:@"isExecuting"];
    [NSThread detachNewThreadSelector:@selector(main) toTarget:self withObject:nil];
    _executing = YES;
    [self willChangeValueForKey:@"isExecuting"];
}

- (void)main {
    NSLog(@"--->concurrentOp-Thread:%@",[NSThread currentThread]);
    [self willChangeValueForKey:@"isExecuting"];
    _executing = NO;
    [self didChangeValueForKey:@"isExecuting"];
    
    [self willChangeValueForKey:@"isFinished"];
    _finished  = YES;
    [self didChangeValueForKey:@"isFinished"];
}
@end

```
- 注意：当operation被cancel时，仍然需要手动出发`isFinished`的`KVO`通知。因为当一个Operation依赖其他Operation时，它会观察所有其他Operation的`isFinished`的值的变化，只有当它依赖的所有Operation的`isFinished`的值为YES时，这个operation才能开始执行。


## NSOperationQueue
- NSOperation配合NSOperationQueue来实现多线程。`NSOperationQueue`对于添加到队列中的操作，首先进入`准备就绪`的状态，然后进入`就绪状态`，`就绪状态`的操作的开始执行顺序由操作之间相对的优先级决定。
- 理论上我们可以创建任意数量的OperationQueue，但数量越多并不意味着我们就能同时执行越多的Operation。**因为并发的Operation数量由系统决定**，系统会根据自身动态调整。
- 操作队列通过设置`maxConcurrentOperationCount`控制**并发、串行**，默认值为-1，并发执行，线程数由系统决定。为1时为串行队列。

队列类型：
- 主队列：`[NSOperationQueue mainQueue];`
- 异步主队列：`[[NSOperationQueue alloc] init];`

### 操作加入到队列
#### addOperation

```
    NSOperationQueue *customQueue = [[NSOperationQueue alloc] init];
    NSInvocationOperation *op1 = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(p_task1) object:nil];
    
    NSBlockOperation *op3 = [NSBlockOperation blockOperationWithBlock:^{
        sleep(2);
        NSLog(@"--->OpQueue1-Thread:%@",[NSThread currentThread]);
    }];
    [customQueue addOperation:op1];
    [customQueue addOperation:op3];
```

#### addOperationWithBlock

```
    NSOperationQueue *customQueue = [[NSOperationQueue alloc] init];
    [customQueue addOperationWithBlock:^{
        NSLog(@"--->BlockOpQueue1-Thread:%@",[NSThread currentThread]);
    }];
```

### 并发控制
operationQueue的并发控制由`maxConcurrentOperationCount`来控制。
- 默认值`NSOperationQueueDefaultMaxConcurrentOperationCount`，-1,系统控制并发数量。
- 设置为1时，串行队列，操作依次执行，但**操作处于的线程由系统控制**。
- `>1` 时，最大并发数不能大于系统能提供的并发数。

### NSOperation操作依赖
> NSOperation依赖关系由自身管理,与Queue无关。
添加是在Operation操作执行之前添加。
_防止循环依赖_

- 添加依赖：`- (void)addDependency:(NSOperation *)op;`
- 移除依赖：`- (void)removeDependency:(NSOperation *)op;`

### NSOperation优先级 - 针对已加入到到队列中的操作有效。
- operation的执行顺序的第一要素是它们的`isReady`状态，其次是它们在队列中的优先级。优先级只决定`isReady`为YES的Operation的执行顺序。当operation依赖未执行完时，`isReady`为NO。
- 队列优先级只应用于相同的`Queue`中的`Operation`之间。
- Operation依赖 > Operation优先级。
- 默认优先级：`NSOperationQueuePriorityNormal`。

```
typedef NS_ENUM(NSInteger, NSOperationQueuePriority) {
	NSOperationQueuePriorityVeryLow = -8L,
	NSOperationQueuePriorityLow = -4L,
	NSOperationQueuePriorityNormal = 0,
	NSOperationQueuePriorityHigh = 4,
	NSOperationQueuePriorityVeryHigh = 8
};
```

### 线程间通信

```
  NSOperationQueue *opQ = [[NSOperationQueue alloc] init];
    [opQ addOperationWithBlock:^{
        sleep(1);
        NSLog(@"--->1-Thread:%@",[NSThread currentThread]);
        [[NSOperationQueue mainQueue] addOperationWithBlock:^{
            NSLog(@"--->2-Thread:%@",[NSThread currentThread]);
        }];
    }];
```
### 线程安全
若每个线程中对全局变量、静态变量只有读操作，而无写操作，一般来说，这个全局变量是线程安全的；若有多个线程同时执行写操作（更改变量），一般都需要考虑线程同步，否则的话就可能影响线程安全。

线程安全解决：**加锁**
- @synchronized
- NSLock、NSRecrusiveLock、NSCondition、NSConditionLock
- pthread_mutex
- dispatch_semaphore
- atomic

性能对比：
![lock_benchmark.png](https://i.loli.net/2018/05/04/5aebfeebd3f99.png)

# 参考
[https://bujige.net/blog/iOS-Complete-learning-NSOperation.html](https://bujige.net/blog/iOS-Complete-learning-NSOperation.html)
[http://blog.leichunfeng.com/blog/2015/07/29/ios-concurrency-programming-operation-queues/](http://blog.leichunfeng.com/blog/2015/07/29/ios-concurrency-programming-operation-queues/)
[https://blog.ibireme.com/2016/01/16/spinlock_is_unsafe_in_ios/](https://blog.ibireme.com/2016/01/16/spinlock_is_unsafe_in_ios/)
