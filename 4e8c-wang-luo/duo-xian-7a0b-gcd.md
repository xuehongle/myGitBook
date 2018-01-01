

GCD全称是Grand Central Dispatch，纯C语言

GCD的优势

GCD是苹果公司为多核的并行运算提出的解决方案

GCD会自动利用更多的CPU内核（比如双核、四核）

GCD会自动管理线程的生命周期（创建线程、调度任务、销毁线程）

### 一.任务和队列

GCD中有2个核心概念

任务：执行什么操作

队列：用来存放任务

#### 1.1任务

1.用同步的方式执行任务

`dispatch_sync(dispatch_queue_t queue, dispatch_block_t block);`

2.用异步的方式执行任务

`dispatch_async(dispatch_queue_t queue, dispatch_block_t block);`

同步和异步的区别

同步：在当前线程中执行

异步：在另一条线程中执行

#### 1.2队列

GCD的队列可以分为2大类型

1.并发队列（Concurrent Dispatch Queue）

可以让多个任务并发（同时）执行（自动开启多个线程同时执行任务）

并发功能只有在异步（dispatch\_async）函数下才有效

2.串行队列（Serial Dispatch Queue）

让任务一个接着一个地执行（一个任务执行完毕后，再执行下一个任务）

容易混淆的术语

有4个术语比较容易混淆：同步、异步、并发、串行

> ps: 我觉得同步异步决定是不是要开线程，串行并发决定要开一条还是多条

#### 1.3主队列和全局队列

1.GCD默认已经提供了全局的并发队列，供整个应用使用，不需要手动创建

使用dispatch\_get\_global\_queue函数获得全局的并发队列

2.主队列是GCD自带的一种特殊的串行队列

放在主队列中的任务，都会放到主线程中执行

使用dispatch\_get\_main\_queue\(\)获得主队列

dispatch\_queue\_t queue = dispatch\_get\_main\_queue\(\);

### 各种队列的执行效果![](/assets/clipboard1.png)二.延时执行和一次性代码执行

iOS常见的延时执行有2种方式

调用NSObject的方法

```
[self performSelector:@selector(run) withObject:nil afterDelay:2.0];
// 2秒后再调用self的run方法
```

使用GCD函数

```
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
// 2秒后异步执行这里的代码...
});
```

一次性代码

使用dispatch\_once函数能保证某段代码在程序运行过程中只被执行1次

```
static dispatch_once_t onceToken;
dispatch_once(&onceToken, ^{
// 只执行1次的代码\(这里面默认是线程安全的\)
});
```

互斥锁真的没有dispatch\_once性能好！单例推荐使用dispatch\_once

### 三.队列组

需求：下载完三张图片后再更新UI

```
dispatch_group_t group = dispatch_group_create();
dispatch_queue_t queue = dispatch_get_global_queue(0, 0);
dispatch_group_async\(group, queue, ^{

    [NSThread sleepForTimeInterval:1.0];

    NSLog(@"下载图片 A, %@", [NSThread currentThread]);

});

dispatch_group_async(group, queue, ^{

    [NSThread sleepForTimeInterval:1.0];

    NSLog(@"下载图片 B, %@", [NSThread currentThread]);

}\);

dispatch_group_async(group, queue, ^{

    [NSThread sleepForTimeInterval:1.0];

    NSLog(@"下载图片 C, %@", [NSThread currentThread]);

});



// 监听工作是异步的！

dispatch_group_notify(group, dispatch_get_main_queue(), ^{

    NSLog(@"三张图片都ok了，可更新UI %@", [NSThread currentThread]);

});

NSLog(@"come here");
```



