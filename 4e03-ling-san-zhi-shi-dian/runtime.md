场景:运行的时候，对某个类动态的填加,修改属性和方法

Demo https://github.com/minggo620/iOSRuntimeLearn

第三方框架一般将kvo封装成block,因为原来的写法确实不太好用

一.objc\_msgSend

- \(void\)viewDidLoad {

    \[super viewDidLoad\];

    Person \*p = \[\[Person alloc\]init\];

//    \[p eat\];

//    \[p performSelector:@selector\(eat\)\];

//    objc\_msgSend\(\) // 苹果不建议使用，所以默认没有后面的参数

    objc\_msgSend\(p, @selector\(eat\)\);

    // 以上三个写法，结果都是p执行eat方法

    //需要selctor加参数的话

    objc\_msgSend\(p, @selector\(sendmsg:\), @"消息内容"\);

}

![](/assets/QQ20171126-200709@2x.png)二.方法欺骗

![](/assets/QQ20171126-211822@2x.png)场景： URLWithString 中文时会为空，并没有判断是否为空，可加个category判断是否为空，这样将URLWithString替换成分类里的XUE\_URLWithString就可以了，但又不想一个一个的替换，还不想导入category头文件，可使用runtime

NSURL \*url = \[NSURL URLWithString:@"www.baidu.com中文"\];

    NSLog\(@"%@", url\);

\#import "NSURL+url.h"

\#import &lt;objc/runtime.h&gt;



@implementation NSURL \(url\)



// 要交换两个方法的时机

+ \(void\)load {

//    class\_getClassMethod 类方法

//    class\_getInstanceMethod 实例方法

    Method urlWithStr = class\_getClassMethod\(\[NSURL class\], @selector\(URLWithString:\)\);

    Method xueurl = class\_getClassMethod\(\[NSURL class\], @selector\(XUE\_URLWithString:\)\);

    method\_exchangeImplementations\(urlWithStr, xueurl\);

}



+ \(instancetype\)XUE\_URLWithString:\(NSString \*\)str {

//    NSURL \*url = \[NSURL URLWithString:str\]; //运行时因为已经替换，所以会进入循环

    NSURL \*url = \[NSURL XUE\_URLWithString:str\];

    if \(url == nil\) {

        NSLog\(@"url空"\);

    }

    return url;

}

@end

三.KVO实现原理

详细见另一篇笔记，IOS 18runtime详解KVO底层实现 - 简书

KVO在\[self.p addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionNew context:nil\]; 之前，p对象的isa是图1，之后就变成了图2

![](/assets/s1.png)

![](/assets/s2.png)

新建了一个Person的子类，重写了子类的setName方法

![](/assets/s3.png)四.归档接档

http://www.jianshu.com/p/8147b3f8dcc3



