# PCH新建

---

xCode6后不再默认建PCH文件了，只好手动添加了。只针对OC程序，Swift我觉得不需要![](/assets/QQ20171231-193813@2x.png)![](/assets/QQ20171231-195644@2x.png)在pch里添加一行代码

`#define kScreenWidth [UIScreen mainScreen].bounds.size.width`

这时，仍不能用，需要手动添加pch的路径![](/assets/QQ20171231-201316@2x.png)上图的路径，改为$\(SRCROOT\)/demo/Demo-PrefixHeader.pch , 路径要正确，可通过showinfinder检查路径是否一致

