>题外话，今天上网的时候发现了一点好玩但是很危险的东西，在此记下来。

######if运算

比如下面这句代码：
	
	if (-0.5 <= x <=0.5) return 0;
	
这句代码真的会返回`0`吗？

答案是否定的，肤浅的阅读这段代码会认为，它用来检查x是不是在[-0.5,0.5]区间内。但并不是这样的。相反这个比较会像这样计算：

	if((-0.5 <= x) <= 0.5) return 0;
	
在C语言中，一个表达式的值是一个整型，要么是`0`，要么是`1`.这是从C没有内建的布尔值类型的时候遗留下来的。所以当`x`和`-0.5`比较的时候，结果是`0`或者`1`，而不是`x`的值。实际上，第二个比较知性起来就像一个相当古怪的否定操作符，也就是说这个if语句的内容只有当`x`值小于`-0.5`的时候才会执行。

######nil的比较

>在Objective-C中，对nil发消息不会发生任何事情，只是简单的返回0.

	[nil isEqual:@"string"];

给nil发消息返回0，在这就相当于NO。这次恰好是正确的答案，所以看起来我们有一个很好的额开头！但是，看看这个：

	[nil isEqual:nil];
	
这个也是返回NO。即使参数是完全相同的值也无关紧要。参数的值到底是什么根本不重要，因为给nil发送消息不管怎样总是返回0。所以用`isEqual`:来判断，`nil`永远不会等同于任何东西，包括它自身。大多情况下这是正确的，但不总是。

在`compare`方法中：

	[nil compare:nil];//返回NSOrderedSame
	[nil compare:@"string"];//返回NSOrderedSame

也就是说，`compare`认为`nil`和任何东西都相等。这是很不正确的逻辑。

>今天的内容可能会比较杂，大家见谅，因为最近在做一个小项目，我会把其中用到的知识时不时整理在笔记中。

##NSURLConnection使用

`NSURLConnection`提供了很多灵活的方法下载URL内容也提供了一个简单的接口去创建和放弃连接，同时使用很多的`delegate`方法去支持连接过程的反馈和控制。

**如何创建一个连接呢？**

为了下载URL内容，程序需要提供一个delegate对象，并且至少实现下面的方法：

	connection:didReceiveResponse:
	connection:didReceiveData:
	connection:didFailWithError:
	connection:DidFinishLoading:
	
举例：

- 创建一个NSURL



- 再通过URL创建`NSURLRequest`,可以指定缓存规则和超时时间



- 创建`NSURLConnection`实例，指定`NSURLRequest`和一个`delegate`对象

如果创建失败，则会返回nil，如果创建成功则创建一个`NSMutableData`实例用来存储数据。

**代码：**

	NSURLRequest *theRequest = [NSURLRequest requestWithURL:[NSStringalloc]initWithFormat:@"http://www.sina.com.cn/]]cachePolicy:NSURLRequestUerProtocolCachePolicy timeoutInterval:60.0]
	
	NSURLConnection *theConncetion=[[NSURLConnection alloc]initWithRequest:theRequest delegate:self];
	
	if(theConnection)
	{
	//创建NSMutableData对象
	receiveData=[[NSMutableData alloc]init];
	}else
	{
	//创建失败
	}

`NSURLConnection`还有几个初始化函数，有个初始化函数可以做到创建连接但是并不马上开始下载，而是通过`start：`开始


当收到`initWithRequst：delegate`消息时，下载会立即开始，在代理收到`connectionDidFinishLoading:`或者`connection:didFailWithError:`消息之前可以通过给连接发送一个`cancel：`消息中断下载。

当下载开始的时候，每当有数据接收，代理会定期收到`connection:didReceiveData:`消息代理应当在实现中存储新接收的数据，下面的例子即是如此：

	-(void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data
	{
	   [receiveData appendData:data];
	}
	
*在上面的方法实现中，可以加入一个进度指示器，提示用户下载进度*。

当下载过程中又错误发生的时候，代理会收到一个`connection:didFailWithError`消息，消息参数里面的`NSError`对象提供了具体的错误细节，他也能提供在用户信息字典里面失败的url请求（使用`NSErrorFailingURLStringKey`）。

**举例：**

	-(void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error
	{
	//[connection release];
	
	[receivedData release];
	NSLog(@"Connection failed!Error - %@ %@",[error localizedDescription],[[error userInfo]objectForKey:NSErrorFailingURLStringErrorKey]);
	}
	
最后，如果连接请求成功的下载，代理会接收`connectionDidFinishLoading：`消息代理不会接收其他消息了。


**举例：**

	-(void)connectionDidFinishLoading:(NSURLConnection *)connection
	{
	//do sonething with the data
	NSLog(@"succeeded %d byte received",[receivedData length]);
	
	}
>以上是因为项目需要登录注册而复习了一下`NSURLConnection`的知识，欢迎大家指正。