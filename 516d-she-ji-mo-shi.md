TODO mvvm

MVC,MVP,MVVM,单例,装饰者模式\(包括Category, Delegate\),观察者模式（包括Notification和Key-Value-Observing\(KVO\)），门面模式（facade外观）,备忘录模式



1.单例

应用场景：UIScreen.main NSUserDefault.stand, UIApplication.shared,NSFileManager.default 

2.装饰者模式\(包括Category, Delegate\)

应用场景：tableview的数据源delegate， AppDelegate

3.观察者模式

应用场景：NSNotifycationCenter, KVO





一.MVC

Model,View,Controller

二.单例

常用: UIScreen.main NSUserDefault.stand, UIApplication.shared,NSFileManager.default 等等

原因: 以NSUserDefault为例，写文件的时候，假若不是单例，则可能同时操作这个plist文件![](/assets/1.png)上图是oc的单例, static 和 gcd的once组成![](/assets/2.png)上图是仿oc单例，![](/assets/3.png)上图class还不支持static,struct支持static![](/assets/4.png)上图class已支持static，变得简单了，init改为private，防止被创建

private override init

三.facade门面模式\(外观模式\)

刘延栋称之为红会模式，例汶川地震捐的钱到了红十字会，红会给了谁，我们并不知道，然后他给了一部分给郭美美，红会就像一个门面，没有这个门面，你想捐钱给郭美美，需要转账红包等方式，或者骑着摩托车送过去，让她开玛莎拉蒂，我们觉得麻烦，没准郭美美也觉得麻烦，所以外观模式很牛.



外观设计模式向复杂的子系统提供了简单的接口

