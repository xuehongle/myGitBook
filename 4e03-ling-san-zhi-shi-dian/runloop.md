[http://www.jianshu.com/p/2d3c8e084205](http://www.jianshu.com/p/2d3c8e084205)

为什么runloop要loop?

* 使程序一直运行，并接受用户输入
* 决定程序在何时应该处理哪些Event
* 调用解耦\(Message Queue\)

runloop In cocoa

* NSTimer
* UIEvent
* AutoRelease
* CADisplayLink
* CATransition
* CAAnimation
* dispatch\__get \_main\_queue_
* NSURLConnection, AFNetworking

Runloop应用

NSTimer

ImageView显示

PerformSelector

常驻线程

自动释放池

![](/assets/1434508-98b48b3de9d15dff.png)

图上的意思：一个线程里一个runloop, 有input源和Timer源，input源有performselector onthread，比方说，其他线程要让主线程做一些事，就是input源，那么Timer源呢，定时检查自己线程的事件，以主线程为例，定时检查是否有点击事件，是否有UI刷新事件。

input源是其他线程的，Timer源是自己线程的

以上博客新增一种情景

timer事件里做耗时操作

![](/assets/QQ20171205-222936@2x.png)

这时scrollview会滑动不顺畅，

怎么解决呢？

对，开个子线程，这样也知道了为什么NSTimer scheduledTimerWithTimeInterval 是用的defaultmode了。

![](/assets/QQ20171205-223516@2x.png)

打印了“子线程”，但timerMethord里的却没有打印，为什么呢？

因为子线程运行完毕了，然后dealloc了。

\(可通过集成NSThread，重写dealloc来证明\)

那么怎样保持子线程的执行呢？

\[\[NSRunLoop currentRunLoop\] run\];

这样，timer的耗时操作也不影响scrollview的滚动，ui也不影响timer

但这样其实还有一个问题，就是这个子线程不会被销毁了。所以可加个标识

![](/assets/QQ20171205-225014@2x.png)

要精准可考虑 GCD timer

