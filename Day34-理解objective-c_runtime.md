#理解OBjective-C Runtime

当刚学Objective-C时，我们最先了解的是Objective-C中用括号包起来的代码像这样..

	[self doSomethingWithVar:var];
	
被转换为：

	objc_msgSend(self,@selector(doSomethingWithVar:),var);
但是，除了这些，我们就不知道之后在运行时做了什么了。

####Objective-C runtime是什么

Objective-C是一个运行时库(Runtime Library)，它是一个主要使用C和汇编写的库，为C添加了面向对象的能力并创造了OBjective-C。这就是说它在类信息(Class Infomation)中被加载，完成所有方法分发，方法转发，等等。Objective-C runtime创建了所有需要的结构体，让Objective-C的面向对象编程变为可能。

####OC Runtime术语

- Instance Method:以‘-’开始
- Class Method:以‘+’开始，比如+(id)alloc


**Selector:**在OC中selector只是一个C的数据结构，用于表示一个你想在一个对象上执行的OC方法，在runtime中的定义像是这样：

	typedef struct objc_selector *SEL;
	
像这样使用：

	SEL aSel=@selector(movieTitle);
	
**Message:**消息由你想要向其发送消息的对象(target)，你想要在上面执行的方法(method)还有你发送的参数(arguments)组成。

	[target getMovieTitleForObject:obj];
	
OC的消息和C函数调用是不同的。事实上，你向一个对象发送消息并不意味着它会执行它。**Object(对象)**会检查消息的发送者，基于这点再决定是执行一个不同的方法函数转发消息到另一个目标对象上。

**Class**如果你查看一个类的runtime信息，你会看到这个...

	typedef struct objc_class *Class;
	typedef struct objc_object{
		Class isa;
	}*id;
	
这个isa指针式当你想对象发送消息时，Objective-C Runtime检查一个对象并且查看它的类是什么然后开始查看它是否响应这些selector所需要做的一切。


####OC Runtime是如何调用你的方法的

在OC中得一个类实现看起来像这样：

	@interface MyClass:NSObject{
		NSInteger counter;
	}
	
	-(void)foo;
	@end
	
	但是runtime不只要追踪这些
	
	#if_OBJC2_
		Class super_class
		const char *name
		long version
		long info
		long instance_size
		struct objc_ivar_list *ivars
		struct objc_method_list **methodLists
		struct objc_cache *cache
		struct objc_protocol_*protocols
	#endif
	
我们可以看到，一个类有其父类的引用，他的名字，实例变量，方法，缓存还有它遵循的协议。runtime在响应类或实力的方法时需要这些信息。

######在objc_msgSend中发生了什么？

事实上在objc_msgSend()中发生了很多事。假设我们有这样的代码...

	[self printMessageWithString:@"Hello World"];
它事实上会被编译器翻译为...

	objc_msgSend(self,@selector(printMessageWithString:),@"Hello World!");
	

下面是在看不懂了，以后再战....

Perference:

[Cocoabit](http://blog.cocoabit.com/blog/2014/10/06/yi-li-jieobjective-cruntime/)