# UIViewController管理

---

## 1.创建一个控制器

控制器常见的创建方式有以下几种

通过storyboard创建

```
UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"Test" bundle:nil];
//初始化“初始控制器”（箭头所指的控制器）,因为stroryboard里可能有多个viewcontroller
NJViewController *nj = [storyboard instantiateInitialViewController];
//通过一个标识初始化对应的控制器
NJViewController *nj = [storyboard instantiateViewControllerWithIdentifier:@”nj"];
```

直接创建

```
NJViewController *nj = [[NJViewController alloc] init];
```

指定xib文件来创建

```
NJViewController *nj = [[NJViewController alloc] initWithNibName:@”NJViewController" bundle:nil];
```

## 2.ViewController的View创建![](/assets/QQ20170725-133203@2x.png)3.控制器view的延迟加载

控制器的view是延迟加载的：用到时再加载

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
self.window = [[UIWindow alloc]initWithFrame:[UIScreen mainScreen].bounds];
ViewController *vc = [[ViewController alloc]init];
    self.window.rootViewController = vc;
    [self.window makeKeyAndVisible]; // 这时候才走loadview方法
}
```

再有一种写法

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
self.window = [[UIWindow alloc]initWithFrame:[UIScreen mainScreen].bounds];
ViewController *vc = [[ViewController alloc]init];
vc.view.backgroundColor = [UIColor blueColor];// 这时候才走loadview方法
    self.window.rootViewController = vc;
    [self.window makeKeyAndVisible]; 
}
```

控制器的view加载完毕就会调用viewDidLoad方法

所以先走loadView方法，然后走viewDidLoad方法

#### 5.ViewController的生命周期![](/assets/生命周期方法.png)5.1启动

```
IOSBaseLearn[8027:2472265] -[AppDelegate application:didFinishLaunchingWithOptions:]
IOSBaseLearn[8027:2472265] -[AppDelegate applicationDidBecomeActive:]
IOSBaseLearn[8027:2472265] -[ViewController viewDidLoad]
IOSBaseLearn[8027:2472265] -[ViewController viewWillAppear:]
IOSBaseLearn[8027:2472265] -[ViewController viewDidAppear:]
```

#### 5.2点home键

ViewController 不走周期方法

#### 5.3 vc1跳到vc2的时候 push

```
One ViewWillDisappear 
Two ViewDidLoad
Two ViewWillAppear
One ViewDidDisappear
Two ViewDidAppear
然后vc2返回到vc1
Two ViewWillDisappear
One ViewWillAppear
Two ViewDidDisappear
One ViewDidAppear
```

#### 5.4 vc1跳到vc2的时候 modal

```
2017-01-07 21:55:36.334176+0800 IOSBaseLearn[6651:487701] -[ModalViewController viewDidLoad]
2017-01-07 21:55:36.335367+0800 IOSBaseLearn[6651:487701] -[OneViewController viewWillDisappear:]
2017-01-07 21:55:36.340717+0800 IOSBaseLearn[6651:487701] -[ModalViewController viewWillAppear:]
2017-01-07 21:55:36.857521+0800 IOSBaseLearn[6651:487701] -[ModalViewController viewDidAppear:]
2017-01-07 21:55:36.857676+0800 IOSBaseLearn[6651:487701] -[OneViewController viewDidDisappear:]
然后返回时
2017-01-07 22:01:17.596701+0800 IOSBaseLearn[6651:487701] -[ModalViewController viewWillDisappear:]
2017-01-07 22:01:17.596927+0800 IOSBaseLearn[6651:487701] -[OneViewController viewWillAppear:]
2017-01-07 22:01:18.104016+0800 IOSBaseLearn[6651:487701] -[OneViewController viewDidAppear:]
2017-01-07 22:01:18.104476+0800 IOSBaseLearn[6651:487701] -[ModalViewController viewDidDisappear:]
```



