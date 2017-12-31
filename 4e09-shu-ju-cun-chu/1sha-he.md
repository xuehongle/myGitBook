# 沙盒

---

## 1.模拟器沙盒路径

Xcode6开始路径发生了变化, bundle路径和沙盒路径分开了![](/assets/QQ20170801-083434@2x.png)![](/assets/QQ20170801-083407@2x.png)

## 2.沙盒结构分析

Documents

* 保存由应用程序产生的文件或者数据，例如：涂鸦程序生成的图片，游戏关卡记录

* iCloud 会自动备份 Document 中的所有文件

  \* 如果保存了从网络下载的文件，在上架审批的时候，会被拒！

tmp

Caches

* 缓存，保存从网络下载的文件，后续仍然需要继续使用，例如：网络下载的离线数据，图片，视频...

* 缓存目录中的文件系统不会自动删除！可以做离线访问！

  \* 要求程序必需提供一个完善的清除缓存目录的"解决方案"！

Preferences

* 系统偏好，用户偏好

* 操作是通过 \[NSUserDefaults standardDefaults\]来直接操作的

图片保存到Caches里，而不是Documents里

![](/assets/QQ20170801-082800@2x.png)![](/assets/QQ20170821-215510@2x.png)常见获取方式

沙盒根目录：

```
NSString *home = NSHomeDirectory();
```

Documents：\(2种方式\)

利用沙盒根目录拼接”Documents”字符串

```
NSString *home = NSHomeDirectory();
NSString *documents = [home stringByAppendingPathComponent:@"Documents"];
// 不建议采用，因为新版本的操作系统可能会修改目录名
```

利用NSSearchPathForDirectoriesInDomains函数

```
// NSUserDomainMask 代表从用户文件夹下找
// YES 代表展开路径中的波浪字符“~”
NSArray *array =  NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, NO);
// 在iOS中，只有一个目录跟传入的参数匹配，所以这个集合里面只有一个元素
NSString *documents = [array objectAtIndex:0];
```

tmp：

```
NSString *tmp = NSTemporaryDirectory();
```

Library/Caches：\(跟Documents类似的2种方法\)

利用沙盒根目录拼接”Caches”字符串

利用NSSearchPathForDirectoriesInDomains函数\(将函数的第2个参数改为：NSCachesDirectory即可\)

