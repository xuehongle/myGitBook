# 移动开发网络杂谈

我想写一篇移动开发的网络杂谈，也不是很全，有需要的再补充。

先瞅一眼网络协议的结构，对整体有一个认识。

![](/assets/20170505181304112.jpeg)

我们平常用的HTTP是应用层的协议。

### 网络闲扯 （没用） {#网络闲扯-没用}

计算机存储数据为什么是0和1？因为是根据纸条打孔的原理做的，但两个计算机之间怎么传输呢，有科学家指出用电线，根据高电平和低电平来代表0和1，但电线信号会衰减，所以有了中继器，放个磁铁在电线旁边，0和1就乱了，所以有了校验。  
后来网线代替了电线，就是所谓的物理层呢。  
交换机工作在数据链路层，而路由器工作在网络层，但现在好多交换机也具备了网络层的功能被叫做三层交换机  
客户端和服务器端通讯时，需要有一定的协议，不然双方不知道在说什么，http和https就是常见的协议

### 一. Http简述 {#一-http简述}

转载请注明出处[ethan\_xue博客](http://blog.csdn.net/ethan_xue)，这是我以前的博客  
HTTP的全称是Hypertext Transfer Protocol，超文本传输协议  
为什么叫协议呢?因为需要客户端和服务器端两边约定一个协议，能互相懂对方的意思  
在HTTP/1.1协议中，定义了8种发送http请求的方法  
GET、POST、OPTIONS、HEAD、PUT、DELETE、TRACE、CONNECT、PATCH  
根据HTTP协议的设计初衷，不同的方法对资源有不同的操作方式

* PUT ：增
* DELETE ：删
* POST：改
* GET：查

然而大家习惯用POST代替PUT 和DELETE 的功能，基本上只剩下GET和POST

数据是2进制数据流，1Byte = 8bit, 8bit可对应ASCII 有256bit, 正好是8bit, 所以通过ASCII能解读出内容是什么，而我们经常看到的是16进制的数据流，因为2进制表示的话比较长

### 二. GET和POST区别 {#二-get和post区别}

2.1 get用于查询数据，post用于提交数据

2.2 get和post请求参数的方式不同，get是[http://www.xxx.com?name=ethan&pwd=123456](http://www.xxx.com/?name=ethan&pwd=123456),这种url是直接可见的，所以安全性比post低，但执行效率比post要高。我觉得get效率高，是因为get没有body,而post有body，需要先处理请求头返回100，然后处理请求体返回200

2.3 get有参数长度限制，（具体的数值取决于浏览器和服务器的限制）post没有，HTTP协议对GET和POST都没有对长度的限制，而对于URL长度上的限制是浏览器跟服务器造成的。

2.4 当然也有说GET和POST本质上就是用socket做的TCP链接，并无差别。但是由于HTTP的规定和浏览器/服务器的限制，导致他们在应用过程中体现出一些不同。 还提到了幂等，这种高大上的概念，意思是请求多次返回的数据是否有区别，get是幂等post是非幂等，愿意研究的可以参考[https://www.zhihu.com/question/28586791](https://www.zhihu.com/question/28586791)

### 三. 请求报文和响应报文 {#三-请求报文和响应报文}

![](/assets/20170505182319864.png)

请求的时候![](/assets/20170830171710402.png)返回数据的时候![](/assets/20170505182434774.png)举个特殊的栗子，有时候需要断点续传，会用到Range请求头 内容是bytes=0-499 //从0到499的头500个字节 bytes=500- //从500字节以后的所有字节

#### 3.1响应状态码 {#31响应状态码}

1xx 消息，一般是告诉客户端，请求已经收到了，正在处理，别急…  
2xx 处理成功，一般表示：请求收悉、我明白你要的、请求已受理、已经处理完成等信息.  
3xx 重定向到其它地方。它让客户端再发起一个请求以完成整个处理。  
4xx 处理发生错误，责任在客户端，如客户端的请求一个不存在的资源，客户端未被授权，禁止访问等。  
5xx 处理发生错误，责任在服务端，如服务端抛出异常，路由出错，HTTP版本不支持等。

#### 3.2.利用cookie保持用户状态 {#32利用cookie保持用户状态}

服务器得知道你要和张三还是李四还是谁会话，所以这里还得提一下session, session表意是会话，cookie是响应头返回的，返回后，将cookie保存在本地。  
cookie保存在客户端，session是保存在服务端，服务器鉴别session需要至少从客户端传来一个session\_id，session\_id通常存于cookie中，这下就理通了

![](/assets/20170505182631588.jpeg)

这种方式是标准的浏览器的那种方式，也可以弄成token，就是唯一标识是服务器创造的，而不是cookie

### 四. post的多种提交形式 {#四-post的多种提交形式}

#### 4.1 application/x-www-form-urlencoded {#41-applicationx-www-form-urlencoded}

这一条是默认的，就是键值对的form表单提交

#### 4.2 application/json {#42-applicationjson}

这个 Content-Type 作为响应头大家肯定不陌生。实际上，现在很多人把它作为请求头，用来告诉服务端消息主体是序列化后的 JSON 字符串。

#### 4.3 Content-Type: multipart/form-data; boundary=—————————7db372eb000e2　 {#43-content-type-multipartform-data-boundary7db372eb000e2}

上传文件的时候Content-Type是这种的

这里简单说几条，还有一些其他的不常用这里就不陈述了

### 五. Https简述 {#五-https简述}

HTTPS（Secure Hypertext Transfer Protocol）安全超文本传输协议  
HTTPS它使用安全套接字层\(SSL\)进行信息交换，简单来说它是HTTP的安全版。  
HTTPS和HTTP的区别主要如下：

* 1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。
* 2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
* 3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
* 4、http的连接很简单，是无状态的；HTTPS协议是由SSL\(后改为TLS\)+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。
* 5、https比http慢

https的安全，我觉得主要是为了防止劫持，TLS比SSL强的地方是在传输层也做了加密。可参考文章末尾的推荐文章：HTTPS 是如何保证安全的？[http://www.jianshu.com/p/b894a7e1c779](http://www.jianshu.com/p/b894a7e1c779)SSL/TLS具有校验机制，一旦被篡改，通信双方会立刻发现。

Charles也可以获得查看数据，需要安装相应的证书.  
Charles的原理

![](/assets/20170719170734204.png)

### 六. TCP,UDP,Socket {#六-tcpudpsocket}

这里提到短连接和长连接，区别：  
长连接能实现服务器主动向客户端发送数据。  
TCP和UDP的区别：  
TCP是建立连接（TCP三次握手）可靠的，UDP是不建立连接 不可靠的，可能会丢包，那UDP存在的意义呢？速度快  
TCP和UDP都是用Socket  
UDP应用场景：多媒体教室/网络流媒体  
TCP应用场景：下载文件时，大文件用udp的话很可能下载不全  
socket\(套接字\)的应用：即时通信（xmpp,mqtt）, 推送，直播，直播时偶尔出现的马赛克就是丢帧udp，打游戏时卡了一下，英雄感觉有了瞬间移动的效果也是丢帧udp的…  
心跳包  
每一个客户端在连接到服务器的时候，都会有一个单独的线程来处理。比如直播时一个房间1000人，那就要有1000个连接保持。这对服务器的负荷是非常大的，但是如果说客户端断开了，服务器还保持着这个线程，就非常的耗资源。客户端不再发心跳包了，说明已经断开了，然后服务器端可直接关掉线程，省资源

### 七. Android和IOS常用网络框架 {#七-android和ios常用网络框架}

Android: 随着Google对HttpClient 摒弃,和Volley的逐渐没落,OkHttp开始异军突起,而Retrofit则对okHttp进行了强制依赖。 还有一些小众的，像XUtils3, NoHttp等  
IOS：ASI因为不再更新而逐渐淡出，较流行的是AFNetworking（OC），Alamofire（swift）

### 参考： {#参考}

HTTPS 是如何保证安全的？[http://www.jianshu.com/p/b894a7e1c779](http://www.jianshu.com/p/b894a7e1c779)

