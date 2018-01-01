# iOS开发中拉伸图片的几种方式

转载自:http://blog.csdn.net/iostendays/article/details/50951624

在iOS开发中,经常会遇到控件尺寸和图片大小不匹配的情况. 一些情况下, 我们需要对图片进行拉伸, 以满足美观需求.

总的来说, 图片的拉伸方式可以分为两种, 一种是通过Xcode自带的Show Slicing功能, 一种是通过代码进行拉伸.

首先, 介绍Xcode自带的Show Slicing 功能.

## 1. 如下图所示的图片, 如果不进行拉伸, 直接设置给一个长度比较长的button以后,其运行效果如图所示.

## ![](/assets/Center.png)

![](/assets/20160322005830160.png)

## 2. 用Show Slicing 进行拉伸.

1\)选中Assets中的图片,右下角有一个Show Slicing ,如图所示.![](/assets/20160322010343475.png)

2\)点击Show Slicing 以后, 会显示如下界面, 点击图中的Start Slicing

 ![](/assets/20160322010559975.png)

3\)拖动虚线,调整拉伸区域, 虚线内的白色区域会被拉伸, 注意要保留住四周的圆角,选择好拉伸区域以后, 点击右下角的Show Overview,就会保存拉伸后的效果了.

那水平方向来说，左边的线表示左边的区域不会被重复，右边的线表示右边的区域不会被重复，中间的线和左边的线之间的区域会被重复。

![](/assets/20160322010823055.png)

4\)这个时候,在给button设置这张背景图片,运行效果如图,这样就比原先美观多了.

![](/assets/20160322011114337.png)

5\)当给UIImageView设置尺寸大小不匹配的背景图片时,还可以通过Stretching 功能,当设置了Image以后, 在Stretching的四个参数中,填入0-1的数值, 调整拉伸效果.

![](/assets/20160322011754574.png)

## 3. 用代码进行拉伸

1\)第一种拉伸方法

```
- (UIImage *)stretchableImageWithLeftCapWidth:(NSInteger)leftCapWidth topCapHeight:(NSInteger)topCapHeight__TVOS_PROHIBITED;
```

使用示例:

```
UIImage *image = [UIImageimageNamed:@"RedButton"];
    image = [image stretchableImageWithLeftCapWidth:image.size.width *0.5topCapHeight:image.size.height *0.5];
    [self.loginButtonsetBackgroundImage:image forState:UIControlStateNormal];
```

   使用效果:

![](/assets/20160322011715386.png)

2\)第二种拉伸方法

```
- (UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsetsNS_AVAILABLE_IOS(5_0);// create a resizable version of this image. the interior is tiled when drawn.
```

使用示例:

```
UIImage *image = [UIImageimageNamed:@"RedButton"];
    image = [image resizableImageWithCapInsets:UIEdgeInsetsMake(image.size.height *0.5, image.size.width *0.5, image.size.height *0.5, image.size.width *0.5)];
    [self.loginButtonsetBackgroundImage:image forState:UIControlStateNormal];
```

使用效果和上图一样

4.在我们经常使用的微信, QQ中,聊天内容会有一个类似于气泡的背景图,如图所示

![](/assets/20160322011633573.png)

有的时候,一次回复了几百个字, 这个图片就会根据内容的多少进行拉伸,要实现这种效果就可以使用代码拉伸的方式.



