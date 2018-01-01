KVC（Key-value coding）键值编码，顾名思义。额，简单来说，是可以通过对象属性名称（Key）直接给属性值（value）编码（coding）“编码”可以理解为“赋值”。这样可以免去我们调用getter和setter方法，从而简化我们的代码，也可以用来修改系统控件内部属性



应用场景: 

1.UITextFile修改placeholder的颜色

\[textField setValue:\[UIColor redColor\] forKeyPath:@"\_placeholderLabel.textColor"\];



https://www.cnblogs.com/MrTao/p/5825457.html

KVC键值查找原理

setValue:forKey:搜索方式

1、首先搜索setKey:方法.\(key指成员变量名, 首字母大写\)

2、上面的setter方法没找到, 如果类方法accessInstanceVariablesDirectly返回YES. 那么按 \_key, \_isKey，key, iskey的顺序搜索成员名.\(NSKeyValueCodingCatogery中实现的类方法, 默认实现为返回YES\)

3、如果没有找到成员变量, 调用setValue:forUnderfinedKey:

valueForKey:的搜索方式

1、首先按getKey, key, isKey的顺序查找getter方法, 找到直接调用. 如果是BOOL、int等内建值类型, 会做NSNumber的转换.

例：搜索-\(NSString\)getName方法，- \(NSString\)name方法，- \(NSString\*\)isName方法

2、上面的getter没找到, 系统也不知道查找的name是什么类型的，那么系统会按照其他数据类型的方式继续找，如数组NSArray，查找countOfKey, objectInKeyAtindex, KeyAtindexes格式的方法. 如果countOfKey和另外两个方法中的一个找到, 那么就会返回一个可以响应NSArray所有方法的代理集合的NSArray消息方法.

3、还没找到, 查找countOfKey, enumeratorOfKey, memberOfKey格式的方法. 如果这三个方法都找到, 那么就返回一个可以响应NSSet所有方法的代理集合.

4、还是没找到, 如果类方法accessInstanceVariablesDirectly返回YES. 那么按 \_key, \_isKey, key, iskey的顺序搜索成员名.

5、再没找到, 调用valueForUndefinedKey.（崩溃）

