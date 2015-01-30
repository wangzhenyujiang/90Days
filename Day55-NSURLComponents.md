#NSURLComponents

当Apple添加一些有趣的类的时候总是大张旗鼓。在iOS7中，Apple 添加了`NSURLComponents`，这个类并没有类参考文档。但是它在“What's New in iOS7”中提到过。

通过使用`NSURLComponents`，把一个URL分成几部分的工作变得很简单。看下面这个栗子：

	NSString *URLString = @"http://en.wikipedia.org/wiki/Special:Search?search=ios";
	
	NSURLCompoents *components=[NSURLComponents componentsWithString:URLString];
	
	NSString *host=components.host;

你也可以用`NSURLCompontents`来构建或者修改网址，比如：

	components.host = @"es.wikipedia.org";
	NSURL *esURL=[components URL];
