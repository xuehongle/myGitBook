Cocoa与Cocoa Touch区别之分是本文要介绍的内容，他们共同点就是二者都包含Objective-C运行时和两个核心框架：

Cocoa包含Foundation和AppKit框架，可用于开发Mac OS X系统的应用程序。

Cocoa Touch包含Foundation和UIKit框架，可用于开发iPhone OS系统的应用程序。

> Cocoa是 Mac OS X 的开发环境，Cocoa Touch是 iPhone OS的开发环境。

框架

Foundation框架实现了NSObjec类（即根类），这个类定义基本对象行为。此外，该框架还实现了用于表示基本类型（例如，字符串和数字）和群体类型（例如，数组和字典）的类，同时也提供一些基本工具，例如用于国际化、对象持久化、文件管理以及XML处理的工具。您还可以使用Foundation框架中的类访问底层系统的实体和服务，例如可以用它来访问端口、线程、锁和进程。Foundation框架以Core Foundation框架为基础，Core Foundation框架提供的是过程化（ANSI C）接口。

您可以使用 AppKit 和UIKit 框架开发应用程序的用户接口。二者用途相同，但是针对平台不同。框架中的类很多，各有不同用途：有的用于事件处理、有的用于画图、有的用于图像处理、有的用于文本处理、有的用于用户排版、还有用于应用程序间数据传输。框架中还包含表视图、滑动条、按键、文本字段以及警告对话框等用户接口元素。

请注意：术语 “Cocoa” 经常被用于泛指所有基于Objective-C运行时且派生自根类（NSObject）的类或对象。

编程语言

Objective-C是开发Cocoa和Cocoa Touch应用程序的本地语言，也是最重要的语言。但是Cocoa和Cocoa Touch应用程序也可以包含C++和ANSI C代码。另外，您也可以使用桥接Objective-C运行时的脚本语言—例如PyObjC和RubyCocoa—开发Cocoa应用程序。

