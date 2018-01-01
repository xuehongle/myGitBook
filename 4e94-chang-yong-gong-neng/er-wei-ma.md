基本上有三大途径：ZBar、ZXing、AVFoundation。

在iOS7苹果也提供AVFoundation支持二维码的扫描。

    ZBar在扫描的灵敏度上和内存的使用上相对于ZXing上都是较优的，但是对于 “圆角二维码” 的扫描确很困难。ZXing 是 Google Code上的一个开源的条形码扫描库，是用java设计的，连Google Glass 都在使用的。但有人为了追求更高效率以及可移植性，出现了c++ port. Github上的Objectivc-C port，其实就是用OC代码封装了一下而已，而且已经停止维护。

ios7以上AVFoundation提供原生api扫描二维码，无论在扫描灵敏度和性能上来说都是最优的，所以毫无疑问我们应该切换到AVFoundation，但是如果需要兼容iOS 6或之前的版本就要使用ZBar或ZXing代替。

官方下载的ZBar SDK竟然是不支持64位的，而苹果早就已经开始对上传到 iTunes 上的应用强制要求支持64位。所以下载网上其他的人编译好的。

