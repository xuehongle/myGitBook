自定义View

> 1.xib方式 2.纯代码方式

## 1.xib方式

创建xib，在xib中拖入需要添加的控件并设置好尺寸。并且要将这个xib的Class设置为我们的自定义类。

通过IBOutlet的方式，将xib中的控件与自定义类进行关联。

## 2.纯代码的方式

initWithFrame:中添加子控件。

layoutSubviews中设置子控件frame。



具体可参照 [https://www.jianshu.com/p/7e47da62899c](https://www.jianshu.com/p/7e47da62899c)

