转载自: http://www.jianshu.com/p/51c13fdec907

新增：

kvo是runtime，生成一个对象的子类，并生成子类对象，并替换原来对象的isa指针，并且重写了set方法。

@interface Person : NSObject



@property \(nonatomic, copy\) NSString \*name;

@property \(nonatomic, strong\) NSMutableArray \*arr;

@property \(nonatomic, strong\) TelPhone \* phone;



- \(void\) eat;

- \(void\) sendmsg: \(NSString \*\)msg;

@end

1.普通属性直接写，就能监听到, 例:name

2.容器类的属性呢？例:arr

\[self.p.arr addObject:@"one"\]; 并不能触发kvo的监听，因为没走set方法。

那么怎样监听容器类的属性的变化呢？

KVO是基于KVC的

 \[\[self.p mutableArrayValueForKey:@"arr"\]addObject:@"two"\]; 就会触发KVO的监听

因为KVC的valueForKey也会创建一个子类，然后重写addObject，同普通属性kvo时重写set方法

3.多级路径属性呢？例:phone

第一种方法: 监听子路径，例: @"phone.name"

\[self.p addObserver:self forKeyPath:@"phone.name" options:NSKeyValueObservingOptionNew context:nil\];

self.p.phone.name = @"iPhone";

第一种方法: 重写Person的keyPathsForValuesAffectingValueForKey方法

+ \(NSSet&lt;NSString \*&gt; \*\)keyPathsForValuesAffectingValueForKey:\(NSString \*\)key {

    NSSet \*keyPaths = \[super keyPathsForValuesAffectingValueForKey:key\];

    

    if \(\[key isEqualToString:@"phone"\]\)

    {

        NSSet \*affectingKeys = \[NSSet setWithObjects:@"\_phone.name", nil\];

        keyPaths = \[keyPaths setByAddingObjectsFromSet:affectingKeys\];

    }

    return keyPaths;

}



---------------------------------------------------------------------------------------------------------------------

前言

KVO: Key-Value-Observer,它来源于观察者模式, 其基本思想\(copy于某度\)是



一个目标对象管理所有依赖于它的观察者对象，并在它自身的状态改变时主动通知观察者对象。这个主动通知通常是通过调用各观察者对象所提供的接口方法来实现的。观察者模式较完美地将目标对象与观察者对象解耦。



本质

当某个类的实例对象的key第一次被观察时，系统就会在运行期动态地创建该类的一个派生类NSKVONotifying\_类名，在这个派生类中重写该类中被观察的属性的 setter 方法。



一步一步验证

  ● ①. KVO的本质就是监听对象的属性进行赋值的时候有没有调用setter方法. 如果有调用setter方法, 就会接收到属性变更的通知, 反之则没有.

我们新建一个类Person, 定义一个属性name, 并自己实现它的setter方法



@property\(nonatomic, copy\) NSString \*name;



- \(void\)setName:\(NSString \*\)name{

    \_name = \[name copy\];    

NSLog\(@"%s", \_\_FUNCTION\_\_\);

}



随后, 我们在ViewController中添加Person的实例对象的属性name的属性监听, 暨添加KVO, 并实现处理变更通知方法 observeValueForKeyPath



 self.p = \[\[Person alloc\] init\]; 

\[self.p addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionNew context:nil\];



- \(void\)observeValueForKeyPath:\(NSString \*\)keyPath ofObject:\(id\)object change:\(NSDictionary \*\)change context:\(void \*\)context{ 

   NSLog\(@"%@, %@", keyPath, self.p.name\);

}



此时, 我们在点击屏幕的时候, 给实例对象p的name进行赋值



- \(void\)touchesBegan:\(NSSet \*\)touches withEvent:\(UIEvent \*\)event{

    static int i = 0;    

self.p.name = \[NSString stringWithFormat:@"name-&gt;%d", i++\];

}



运行程序, 点击两次屏幕, 我们可以可以观察到打印如下



-\[Person setName:\]name, name-&gt;0 

-\[Person setName:\]name, name-&gt;1



我们再进行一次反证, 我们不使用setter方法对name进行赋值, 看是否属性的值改变会被监听



我们将Person.h修改如下



@interface Person : NSObject{

@public NSString \*\_name;

}

@property\(nonatomic, copy\) NSString \*name;@

end



此时, @property只会帮我们生成getter方法\(setter已经被我们自己实现了的\), 不会生产\_name成员变量



我们修改点击屏幕的代码如下



- \(void\)touchesBegan:\(NSSet \*\)touches withEvent:\(UIEvent \*\)event{

    static int i = 0;

    self.p-&gt;\_name = \[NSString stringWithFormat:@"name-&gt;%d", i++\];}



此时我们运行程序会发现, 不管怎么点击屏幕, 控制台都不会有任何打印.



结论: KVO的本质就是监听对象的属性进行赋值的时候有没有调用\`setter\`方法



② 当某个类的实例对象的key第一次被观察时,系统就会在运行期动态地创建该类的一个派生类NSKVONotifying\_类名，在这个派生类中重写该类中被观察的属性的 setter 方法。



如果我们要重写方法, 一般都会调用\[super 方法名\], 那么系统是怎么做到重写属性的setter方法, 且通知观察者Observer的呢? 下面我们一步一步来进行分析:![](/assets/61432B61A2E843A989AF9DE59BFB8F10.jpeg)

如图所示, 我们有断点A和B, 此时我们运行程序, 程序在停留在断点A处, 我们观察isa指针

![](/assets/67D7C6E0469F47C48E7E55705520CD36.jpeg)

此时,isa指针指向Person类

我们跳过这个断点, continue, 并点击屏幕, 此时程序停留到了断点B, 此时我们再次观察isa

![](/assets/F2D8E01B57F243B79F411F6C5F80EF7C.jpeg)

此时isa指针被系统动态的指向了派生类NSKVONotifying\_Person. 由此, 结论被证.



自己实现KVO

由于系统是自动实现的派生类NSKVONotifying\_Person, 这儿我们自己手动创建一个派生类ALINKVONotifying\_Person, 集成自Person. 同时给NSObject创建一个分类, 让每一个对象都拥有我们自定义的KVO特性.



NSObject+KVO.h



\#import 

@interface NSObject \(KVO\)

- \(void\)alin\_addObserver:\(NSObject \*\)observer forKeyPath:\(NSString \*\)keyPath options:\(NSKeyValueObservingOptions\)options context:\(nullable void \*\)context;

@end



NSObject+KVO.m



\#import "NSObject+KVO.h"

\#import "ALINKVONotifying\_Person.h"

\#import 

NSString \*const ObserverKey = @"ObserverKey";

@implementation NSObject \(KVO\)

// 仿系统的, 前缀是为了区别系统的

- \(void\)alin\_addObserver:\(NSObject \*\)observer forKeyPath:\(NSString \*\)keyPath options:\(NSKeyValueObservingOptions\)options context:\(nullable void \*\)context{    // 把观察者保存到当前对象    objc\_setAssociatedObject\(self, \(\_\_bridge const void \*\)\(ObserverKey\), observer, OBJC\_ASSOCIATION\_RETAIN\_NONATOMIC\);    // 修改对象isa指针    object\_setClass\(self, \[ALINKVONotifying\_Person class\]\);}@end



ALINKVONotifying\_Person.m



\#import "ALINKVONotifying\_Person.h"\#import extern NSString \*const ObserverKey;@implementation ALINKVONotifying\_Person- \(void\)setName:\(NSString \*\)name{    \[super setName:name\];    // 获取观察者    id obsetver = objc\_getAssociatedObject\(self, ObserverKey\);    \[obsetver observeValueForKeyPath:@"name" ofObject:self change:nil context:nil\];}@end



此时我们调用自己定义的监听方法, 效果和系统的也是一样的



\[self.p alin\_addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionNew context:nil\];

