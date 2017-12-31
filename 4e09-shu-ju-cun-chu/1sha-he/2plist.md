## 1.plist

属性列表是一种XML格式的文件，拓展名为plist

如果对象是NSString、NSDictionary、NSArray、NSData、NSNumber等类型，就可以使用writeToFile:atomically:方法直接将对象写到属性列表文件中

将一个NSDictionary对象归档到一个plist属性列表中

// 将数据封装成字典

NSMutableDictionary \*dict = \[NSMutableDictionary dictionary\];

\[dict setObject:@"母鸡" forKey:@"name"\];

\[dict setObject:@"15013141314" forKey:@"phone"\];

\[dict setObject:@"27" forKey:@"age"\];

// 将字典持久化到Documents/stu.plist文件中

\[dict writeToFile:path atomically:YES\];



plist的缺点： plist只能存储系统自带的一些常规的类, 也就是有writeToFile方法的对象才可以使用plist保存数据



dictionary有自定义对象也不能生成plist文件

NJPerson \*p = \[\[NJPerson alloc\] init\];

  p.name =@"lnj";

 NSDictionary \*dict = @{@"person": p};

  \[dict writeToFile:path atomically:YES\];  //不会生成plist文件



## 2.偏好设置

NSUserDefaults \*defaults = \[NSUserDefaults standardUserDefaults\];

\[defaults setObject:@"itcast" forKey:@"username"\];

\[defaults setFloat:18.0f forKey:@"text\_size"\];

\[defaults setBool:YES forKey:@"auto\_login"\];



读取上次保存的设置

NSUserDefaults \*defaults = \[NSUserDefaults standardUserDefaults\];

NSString \*username = \[defaults stringForKey:@"username"\];

float textSize = \[defaults floatForKey:@"text\_size"\];

BOOL autoLogin = \[defaults boolForKey:@"auto\_login"\];



## 3.归档

NSKeyedArchiver

NSCoding协议有2个方法：

encodeWithCoder:

initWithCoder:

## 4.Sqlite3 和CoreData

可使用fmdb或者wcdb

