- PHP 优化，opcahche 是怎么用的
	1. PHP来说，PHP并不产生机器码，
		解析后产生中间码opCode，
		然后Zend引擎解析执行opCode

	2. 如果PHP源码没有发生变化，
		那么最终执行的opCode也不会变化，
		没有必要每次都重新生成opCode。
		我们可以把opCode缓存下来
	
	3. Opcode cache 的目地是避免重复编译，
		减少 CPU 和内存开销

	4. PHP7之后，已经内置了opcache拓展，
		但是没有开启，需要在php.ini中添加

			opcache.enable = 1

	5. 注意事项

		a. 不建议在开发过程中开启Opcache
		b. 不建议将Opcache指标设置太大

- opcode优化

	TODO

	没搞懂原理

	先答上面这个试试，如果要求很高再重新系统学学

- cgi 是什么东西，用来干嘛的
- PHP-FPM 是什么架构模式的，你怎么优化过它
	
	1. 多进程架构模式
	2. 主要就是由master进程管理worker进程
	3. worker进程负责客户端的请求监听和处理
	4. 子进程挂掉之后，如果我们配置了static模式，
		aster进程会重新启一个新的worker进程
	5. 有三种模式：static、dynamic、ondemand

		static: 始终保持固定数量的worker进程数，
			由pm.max_children决定，不会动态扩容

		dynamic: php-fpm启动时，会初始启动一些worker，
			初始启动worker数决定于pm.max_children的值
			在运行过程中动态调整worker数量，worker的数量受限
			于pm.max_children配置，同时受限全局配置process.max。

		ondemand:  不会启动任何一个worker，而是按需启动，
			只有当连接过来的时候才会启动
	
	6. 没做过优化，只是在出账的时候会将设置的woker数量开
		多一点，加快出账速度

- 传值与传引用

	传值会: 很耗时间,特别是对于大型的字符串和对象来说，
		这将会是一个代价很大的操作, 但是相对来说，操作安全

	传送引用: 函数内的任何操作等同于对传送变量的操作，
		传送大型变量时效率高！但是可能会影响GC，因为引用计数加1了

- CGI、FastCGI、PHP-FPM 关系图解

	CGI: 通用网关接口协议，是 apache、nginx这种web server 
		与 PHP或其他语言之间构建的web app进行交互的一种协议。
		主要就是传递 URL、查询字符串、POST数据、HTTP header等数据。
		CGI协议问题的就是每一次web请求都会有启动和退出过程, 
		也就是最为人诟病的fork-and-execute模式，这样一在大规模并发下，就死翘翘了。

	FastCGI: FastCGI也可以说是一种协议，是用来提高CGI程序性能的
		像是一个常驻(long-live)型的CGI。
		当客户端请求到达Web Server时，FastCGI进程管理器选择并连接到一个CGI解释器
		Web server将CGI环境变量和标准输入发送到FastCGI子进程php-cgi。
		FastCGI子进程完成处理后，将标准输出和错误信息从同一连接返回Web Server。
		当FastCGI子进程关闭连接时，请求便告处理完成。
		FastCGI子进程接着等待，并处理来自FastCGI进程管理器(运行在Web Server中)的下一个连接

	PHP-FPM:
		
		这是一个PHP专用的 PHP fastcgi 管理器, 是FastCGI协议的一个具体实现
		他负责管理一个进程池，来处理来自Web服务器的请求，支持多进程管理
		支持平滑重启。

- PHP 的垃圾收集机制

	每个php变量存在一个叫"zval"的变量容器中。一个zval变量容器，
	除了包含变量的类型和值，还包括两个字节的额外信息。
	第一个是"is_ref"，是个bool值，用来标识这个变量是否是属于引用集合(reference set).

	第二个额外字节是"refcount"，用以表示指向这个zval变量容器的
	变量(也称符号即symbol)个数。所有的符号存在一个符号表中，
	其中每个符号都有作用域(scope)，那些主脚本(比如：通过浏览器请求的
	的脚本)和每个函数或者方法也都有作用域。

	就是说，仅仅在引用计数减少到非零值时，才会产生垃圾周期(garbage cycle).
	为避免不得不检查所有引用计数可能减少的垃圾周期，这个算法把所有可能根
	放在根缓冲区(root buffer)中.可以同时确保每个可能的垃圾根(possible garbage root)
	在缓冲区中只出现一次. 当垃圾回收机制打开时，每当根缓存区存满时，就会执行循环查找算法。
	判断是以下两种情况之后就会进行释放：
		目前垃圾只会出现在数组和对象两种类型中，数组的情况上面已经介绍了，
		对象的情况则是成员属性引用对象本身导致的

	null：将null赋值给一个变量是直接将该变量指向的数据结构置空，同时将其引用计数归0。

- CLI 模式的生命周期

	** https://upload-images.jianshu.io/upload_images/11224747-cbcdd660fb0f2a63.png **

- 内存分配流程
- php 数组的实现
- 依赖注入
- Print、echo、print_r有什么区别？
- SESSION与COOKIE的区别？
- PHP处理数组的常用函数？（重点看函数的‘参数’和‘返回值’）
- PHP处理字符串的常用函数？（重点看函数的‘参数’和‘返回值’）
- PHP处理时间的常用函数？（重点看函数的‘参数’和‘返回值’）
- PHP操作文件的常用函数？（重点看函数的‘参数’和‘返回值’）
- PHP处理数据库的常用函数？（重点看函数的‘参数’和‘返回值’）
- 简述 private、 protected、 public修饰符的访问权限。
- 堆和栈的区别？
- 什么是构造函数，什么是析构函数，作用是什么？
- 如何重载父类的方法，举例说明
- 常用的魔术方法有哪些？举例说明
- $this和self、parent这三个关键词分别代表什么？在哪些场合下使用？
- 类中如何定义常量、如何类中调用常量、如何在类外调用常量。
- 作用域操作符::如何使用？都在哪些场合下使用？
- __autoload()方法的工作原理是什么
- PHP 页面重定向的方法有哪些;
- PHP 做好防盗链的基本思想 防盗链
- require，include区别

	

- PHP写出显示客户端IP与服务器IP的代码

	$_SERVER["REMOTE_ADDR"];

	gethostbyname($_SERVER["SERVER_NAME"]);

	需要注意的就是Nginx转发的话，配置不对会拿到错误的IP，
	还需要使用PHP进行特殊的处理

- isset(),empty()的区别

	1. empty()函数是用于确定变量是否为空；
		换句话说，如果变量是空字符串，false，
		array（），NULL，“0”，0和未设置的变量，它将返回true。

	2. 如果为NULL则返回false

- 在PHP中error_reporting这个函数有什么作用?

	设置脚本运行时的错误报告级别，并返回当前的错误级别

- 单引号和双引号的区别是什么？

	1. 双引号串中的内容可以被解释而且替换,
		而单引号串中的内容总被认为是普通字符

- 什么是对象克隆？

	1. 对象复制可以通过 clone 关键字来完成
		对对象里面的属性会进行深复制，里面的对象
		会进行浅复制
	
	2. 在PHP中默认是浅克隆，要想实现深克隆，
		就需要对该类对象使用魔术方法__clone(),
		并在里面实现深度克隆。
		(clone 关键字调用的时候会调用这个方法)

- 通过哪一个函数，可以把错误转换为异常处理？

	set_error_handler(callable $error_handler [, int $error_types = E_ALL | E_STRICT ])
	ror_handler(callable $error_handler [, int $error_types = E_ALL | E_STRICT ])

- 合并两个数组有几种方式，试比较它们的异同

	1. arrary_merge()

		但是对于键值对的数组来说，如果有相同的键，那么第二个数组会覆盖第一个数组相同的键所对应的值。

	2. " + "
		
		可以看到，对于用"+"来合并两个数组而言，
		无论是普通数组还是键值对型数组，
		只要下标相同或者键相同，
		都是前者覆盖后者。这一点需要注意。
	
	3. array_combine()
		
		函数会得到一个新数组，它由一组提交的键和对应的值组成

		(
			如果需要合并数组成为上面的形式，
			那么合并的两个数组的长度必须相等，
			也就是count($arr1) == count($arr2)，并且不能为空
		)

- 请写一个函数来检查用户提交的数据是否为整数

	isnumeric()

- PHP7的新特性？
	
	1. 性能提升

		a. 变量存储字节减小，减少内存占用，提升变量操作速度
		b. 改善数组结构，数组元素和hash映射表被分配在同一块内存里，
			降低了内存占用、提升了 cpu 缓存命中率
		c. 改进了函数的调用机制，通过优化参数传递的环节，
			减少了一些指令，提高执行效率

	2. 以前的许多致命错误，现在改成抛出异常
	3. PHP 7.0比PHP5.0移除了一些老的不在支持的SAPI（服务器端应用编程端口）和扩展
	4. PHP 7.0比PHP5.0新增加了函数的返回类型声明
	5. PHP 7.0比PHP5.0新增加了标量类型声明
	6. PHP 7.0比PHP5.0新增了空接合操作符。
	7. PHP 7.0比PHP5.0新增加了结合比较运算符。
	10. PHP 7.0比PHP5.0新增加匿名类。

- PHP“foreach”实际上是如何工作的？
	
	1. foreach遍历数组的顺序是由元素的添加顺序决定的
	
	2. 当 foreach 开始执行时，数组内部的指针会自动指向第一个单元,
		并且试图努力的往后移动指针，直到移出界.
		结束foreach结束后，并没有帮我们把指针初始化.
		（在这个地方常常有坑）

- 鸟哥PHP优化建议
	
	** https://www.laruence.com/2015/12/04/3086.html **
