## 一、NSThread的初始化

1.动态方法

```
- (id)initWithTarget:(id)target selector:(SEL)selector object:(id)argument;  

// 初始化线程  
  NSThread *thread = [[NSThread alloc] initWithTarget:self selector:@selector(run) object:nil];  
  // 设置线程的优先级(0.0 - 1.0，1.0最高级)  
  thread.threadPriority = 1;  
  // 开启线程  
  [thread start];
```

参数解析：

selector ：线程执行的方法，这个selector最多只能接收一个参数

target ：selector消息发送的对象

argument : 传给selector的唯一参数，也可以是nil

2.静态方法

```
+ (void)detachNewThreadSelector:(SEL)selector toTarget:(id)target withObject:(id)argument;  

[NSThread detachNewThreadSelector:@selector(run) toTarget:self withObject:nil];  
// 调用完毕后，会马上创建并开启新线程
```

3.隐式创建线程的方法

```
[self performSelectorInBackground:@selector(run) withObject:nil];
```

## 二、获取当前线程

NSThread \*current = \[NSThread currentThread\];

## 三、获取主线程

NSThread \*main = \[NSThread mainThread\];

## 四、暂停当前线程

```
  // 暂停2s  
[NSThread sleepForTimeInterval:2];  

 // 或者  
 NSDate *date = [NSDate dateWithTimeInterval:2 sinceDate:[NSDate date]];  
[NSThread sleepUntilDate:date];
```

## 五、线程间的通信

1.在指定线程上执行操作

```
[self performSelector:@selector(run) onThread:thread withObject:nil waitUntilDone:YES];
```

2.在主线程上执行操作

```
[self performSelectorOnMainThread:@selector(run) withObject:nil waitUntilDone:YES];
```

3.在当前线程执行操作

```
[self performSelector:@selector(run) withObject:nil];
```

## 六、优缺点

1.优点：NSThread比其他两种多线程方案较轻量级，更直观地控制线程对象

2.缺点：需要自己管理线程的生命周期，线程同步。线程同步对数据的加锁会有一定的系统开销，无法对线程进行更详细的设置

