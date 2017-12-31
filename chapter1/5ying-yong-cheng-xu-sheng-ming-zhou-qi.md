# AppDelegate类

---

![](/assets/wpsB392.tmp.jpeg)

启动程序

```
2014-07-28 15:22:39.883 LifeCycle[3024:a0b] didFinishLaunchingWithOptions
2014-07-28 15:22:39.887 LifeCycle[3024:a0b] DidBecomeActive
```

按下Home键

```
2014-07-28 15:22:43.130 LifeCycle[3024:a0b] WillResignActive
2014-07-28 15:22:43.131 LifeCycle[3024:a0b] DidEnterBackground /// 进入后台（home键、锁屏）
```

重新点击程序

```
2014-07-28 15:22:44.380 LifeCycle[3024:a0b] WillEnterForeground /// 回到前台（激活、解锁）
2014-07-28 15:22:44.380 LifeCycle[3024:a0b] DidBecomeActive
```

内存清除——应用终止场景

应用在后台处理完成时进入挂起状态（这是一种休眠状态），如果这时发出低内存警告，为了满

足其他应用对内存的需要，该应用就会被清除内存从而终止运行

内存清除有两种情况，可能是系统强制清除内存，也可能是由使用者从任务

栏中手动清除（即删掉应用）

在内存清除场景下，应用不会调用任何方法，也不会发出任何通知。

