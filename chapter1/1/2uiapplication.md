# UIApplication

---

> UIApplication是main函数里创建的，是程序的第一个对象，UIApplication会用主线程的Runloop, 去监听系统事件，保证程序不退出。

UIApplication对象是应用程序的象征;

1. 每一个应用都有自己的UIApplication对象,而且是单例的;
2. 通过\[UIApplication sharedApplication\]可以获得这个单例对象;
3. 一个iOS程序启动后创建的第一个对象就是UIApplication对象;
4. 利用UIApplication对象,能进行一些应用级别的操作.

#### 常用属性

1.设置应用程序图标右上角的红色提醒数字:

```
UIApplication * app = [UIApplication sharedApplication];
app.applicationIconBadgeNumber = 123;
//    iOS8以后需要注册,才能将未读的数在图标右上角显示
```

2.设置联网指示器\(菊花\)的可见性

```
app.networkActivityIndicatorVisible = YES;
```

3.打电话，发短信，发邮件，打开网页等

```
//    打电话UIApplication *app = [UIApplication sharedApplication];[app openURL:[NSURL URLWithString:@"tel://10086"]];
//    发短信[app openURL:[NSURL URLWithString:@"sms://10086"]];
//    发邮件[app openURL:[NSURL URLWithString:@"mailto://12345@qq.com"]];
//    打开一个网页资源[app openURL:[NSURL URLWithString:@"https://m.baidu.cn"]];
```

4.状态栏 （不建议）  

// 从iOS7开始，系统提供了2种管理状态栏的方式

// 通过UIViewController管理（每一个UIViewController都可以拥有自己不同的状态栏）

// 通过UIApplication管理（一个应用程序的状态栏都由它统一管理）

// 在iOS7中，默认情况下，状态栏都是由UIViewController管理的，UIViewController实现下列方法就可以轻松管理状态栏的可见性和样式

// 状态栏的样式- \(UIStatusBarStyle\)preferredStatusBarStyle;

// 状态栏的可见性\(BOOL\)prefersStatusBarHidden;

// 想利用UIApplication来管理状态栏，首先得修改Info.plist的设置

