# CoreAnimation -核心动画

> Core Animation是直接作用在CALayer上的\(并非UIView上\)非常强大的跨Mac OS X和iOS平台的动画处理API，Core Animation的动画执行过程都是在后台操作的，不会阻塞主线程。

核心动画继承结构

![](/assets/540664-45b078efac175dbb.png)

注意：核心动画中的虚类不能使用，而应该使用他们子类中的实类。



初始化一个CAAnimation对象，并设置一些动画相关属性

通过调用CALayer的addAnimation:forKey:方法，增加CAAnimation对象到CALayer中，这样就能开始执行动画了

通过调用CALayer的removeAnimationForKey:方法可以停止CALayer中的动画



核心动画给我们展示的只是一个假象，layer的的frame、bounds、position并不会在动画完毕之后发生改变。

UIView封装的动画，会使会真实修改view的一些属性。



