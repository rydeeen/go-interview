- 基础知识

1. 计算机网络应用层协议有哪些
	
	HTTP
	Websocket
	SFTP
	FTP
	SMTP

2. 介绍一下子网掩码

	主要是为了将某个IP地址划分成网络地址和主机地址两部分。

3. 谈谈rpc是怎么实现的

	RPC（Remote Procedure Call）-远程过程调用，
	他是一种通过网络从远程计算机程序上请求服务，
	而不规定底层网络技术的协议。

	我认为RPC是一种为实现远程调用而提出一种思想，
	至于你用什么方式去达到目的都可以。

	常见的RPC方式有很多种：
		
		GRPC -----> http2.0 传送 protobuffer
		thrift -----> 使用thrift，不能与http等其他协议兼容
		还有一下是使用XML序列化协议传输的

4. 说说7层网络，说说应用层和传输层都有什么协议

	物理层
	数据链路层
	网络层
	连接层
	传输层
	表示层
	会话层
	应用层

- http

http协议的methods有哪些。

	GET
	POST
	PUT
	DELETE
	HEAD
	OPTIONS
	PATCH
	CONNECT
	TRACE

讲一下get和post的区别

	从浏览器的角度：
		GET会有长度限制
		会将参数显示，POST不会
		GET没有body，POST有

	但是对于我们程序猿，不应该有这种想法：
		
		在传输的角度上，GET POST区别不大，
		都没有特定的限制，
		如果抓包，都能将内容解析（要安全，还是得上SSL）

http的请求头和boby用什么来区分。 

	空行

http的请求头可以携带二进制数据吗

	不可以，得用 ASCII

http长连接怎么保持

	onnection： keep-alive，

Http quic听过吗

	TODO

xss听过吗

	TODO

http 1.0 1.1 2.0区别

	<< HTTP1.0和HTTP1.1的一些区别 >>

	缓存处理
		在HTTP1.0中主要使用header里的If-Modified-Since,Expires来做为缓存判断的标准，
		HTTP1.1则引入了更多的缓存控制策略例如Entity tag，If-Unmodified-Since, 
		If-Match, If-None-Match等更多可供选择的缓存头来控制缓存策略。

	带宽优化及网络连接的使用
		HTTP1.0中，存在一些浪费带宽的现象，例如客户端
		只是需要某个对象的一部分，而服务器却将整个对象送过来了，并且不支持断点续传功能，
		HTTP1.1则在请求头引入了range头域，它允许只请求资源的某个部分，
		即返回码是206（Partial Content），这样就方便了开发者自由的选择以便于充分利用带宽和连接。

	错误通知的管理
		在HTTP1.1中新增了24个错误状态响应码，
		如409（Conflict）表示请求的资源与资源的当前状态发生冲突；
		410（Gone）表示服务器上的某个资源被永久性的删除。

	Host头处理
		在HTTP1.0中认为每台服务器都绑定一个唯一的IP地址，
		因此，请求消息中的URL并没有传递主机名（hostname）。
		但随着虚拟主机技术的发展，在一台物理服务器上可以存在多个虚拟主机（Multi-homed Web Servers），
		并且它们共享一个IP地址。HTTP1.1的请求消息和响应消息都应支持Host头域，
		且请求消息中如果没有Host头域会报告一个错误（400 Bad Request）。

	长连接
		HTTP 1.1支持长连接（PersistentConnection）和请求的流水线（Pipelining）处理，
		在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟，
		在HTTP1.1中默认开启Connection： keep-alive，一定程度上弥补了HTTP1.0每次请求都要创建连

	<< HTTP2.0和HTTP1.X相比的新特性 >>

	新的二进制格式（Binary Format）
		HTTP1.x的解析是基于文本。基于文本协议的格式解析存在天然缺陷，
		文本的表现形式有多样性，要做到健壮性考虑的场景必然很多，二进制则不同，
		只认0和1的组合。基于这种考虑HTTP2.0的协议解析决定采用二进制格式，实现方便且健壮。

	多路复用（MultiPlexing）
		即连接共享，即每一个request都是是用作连接共享机制的。
		一个request对应一个id，这样一个连接上可以有多个request，
		每个连接的request可以随机的混杂在一起，接收方可以根据request的
		id将request再归属到各自不同的服务端请求里面。

	header压缩	
		如上文中所言，对前面提到过HTTP1.x的header带有大量信息，
		而且每次都要重复发送，HTTP2.0使用encoder来减少需要传输的header大小，
		通讯双方各自cache一份header fields表，
		既避免了重复header的传输，又减小了需要传输的大小。

	服务端推送（server push）
		同SPDY一样，HTTP2.0也具有server push功能。	

输入url的整个过程

	TODO

为什么使用http而不使用grpc

	TODO

httprestful的定义规范  
HTTP状态码

    100
    101
    200
    201
    202
    400
    401
    403
    404
    300
    301
    302
    500
    501
    502
    503

TCP
    Tcp有哪些状态
    三次握手流程
    第三次的ack丢失会怎么办
    Time_wait过多怎么办
    Tcp和udp的区别
    aceept发生在第几次
    Tcp怎么保证顺序
    tcp为什么试三次握手
    time_wait为什么会等待2msl
    客户端4次挥手后处于什么状态，为什么是2msl
    大量出现CLOSE_WAIT是因为什么，怎么办，大量出现TIME_WAIT呢
    tcp如何保证稳定性
    TCP流量控制、拥塞控制 
    TCP半连接队列
    TCP滑动窗口

websocket
    websocket的稳定性是如何做的？ 
    海外机器的延迟如何保证 
    为什么会选用websocket？ 
    能对比一下websocket、长连接、EventSource的优缺点吗 
    在前端如何处理websocket兼容性问题？ 


