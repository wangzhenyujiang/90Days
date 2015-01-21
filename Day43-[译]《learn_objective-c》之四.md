#[译]《Learn Objective-C》之四

银生的第一篇翻译，虽然短，但是错误多啊，望大家多指正。

##Basic Memory Management

如果你正在写一个Mac OS X上的应用，你有权利选择是否使用垃圾回收机制。一般来说，这意味着当你遇到一些复杂情况的时候，不用去想内存管理的事情。

当然，你不一定总是在允许垃圾回收机制的环境下工作的。以防万一，你需要去知道一些基础的概念。

如果你用`alloc`的方式新建了一个对象，那么以后你需要去`release`掉这个对象。你不应该人为地`release`掉一个`autoreleased`对象因为如果这样的话你的应用会crash掉。

这里两个栗子：

	//string1 will be released automatically
	NSString* string1=[NSString string];
	
	//must release this when done
	NSString* string2=[[NSString alloc]init];
	[string2 release];
对于这个教程，你可以这样认为，当一个函数结束的时候，一个局部对象会被释放掉。