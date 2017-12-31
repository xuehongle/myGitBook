# UIWindow

---

UIWindow是一种特殊的UIView，通常在一个app中只会有一个UIWindow

iOS程序启动完毕后，创建的第一个视图控件就是UIWindow

添加UIView到UIWindow中两种常见方式:

```
- (void)addSubview:(UIView *)view;
```

直接将view添加到UIWindow中，但并不会理会view对应的UIViewController

```
@property(nonatomic,retain) UIViewController *rootViewController;
```

自动将rootViewController的view添加到UIWindow中，负责管理rootViewController的生命周期



这两种方式是有区别的，addSubview只是添加了view, 但本应该viewcontroller管理的事件就没有了

UIWindow的获得:

```
[UIApplication sharedApplication].windows
[UIApplication sharedApplication].keyWindow
view.window
//获得某个UIView所在的UIWindow
```



