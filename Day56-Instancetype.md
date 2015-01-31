#instancetype
这个知识点是在《iOS7 Programming Pushing The Limits》上看到的，但是上面的解释让人迷惑。在网上很幸运的搜到了[高见龙](http://blog.eddie.com.tw/2013/12/16/id-and-instancetype/)大神对这一知识点的总结，而且[NSHipster](http://nshipster.cn/instancetype/)上对此知识点也提到过。



如果我们去看一下`NSObjet`上`alloc`跟`init`的定义：

	@interface NSObject <NSObject>
	{
		Class isa OBJC_ISA_AVAILABILITY;
	}
	
	- (id)init;
	+ (id)new;
	+ (id)alloc;
	
会发现`alloc`跟`init`的回传类型都是`id`。而大家又知道`id`是一个可以指向任意对象的指针。下面是`id`的定义：

	//A pointer to an instance of a class
	typedef struct objc_object *id;
	
所以，如果这样的话：

	NSArray* myArray=[[NSArray alloc] init];

看起来好像没有问题，执行起来也正常，但是这里就有问题了..既然`alloc`和`init`都是返回的`id`类型的，Objective-C是个动态语言，那编译器怎么知道它应该是个`NSArray`呢？

根据[clang的文件](http://clang.llvm.org/docs/LanguageExtensions.html)说明，当我们写`[NSArray alloc]`的时候，消息接收者receiver`NSArray`收到消息，但是并不是乖乖的就只回传`id`型别，而是回传receiver型别，在这个例子就是`NSArray`。同理，`init`也是一样的，所以`[[NSArray alloc] init]` 返回的是一个`NSArray`型别。**在OBjective-C中，`alloc` `init` `new` 等方法都享有这个特别的服务**。

但是如果不是这些享有特别服务的方法呢？

	@interface Animal : NSObject
	+ (id)createAnimal;
	@end
	
	@implementation Animal
	+ (id)creareAnimal
	{
		return [[self alloc] init];
	}
	@end
	
	
	@interface Fox : Animal
	-(void)say;
	@end
	
	@implementation Fox
	-(void)say
	{
		NSLog(@"what does the fox say!?");
	}
	@end
	

如果我们使用这个类的时候不小心写成：

	[[Animal createAnimal] say];

编译器并不会报错，但是当你执行起来的时候就会crash掉。我们通过肉眼就能发现crash的原因，父类`Animal`根本没有定义`say`方法。但是这么明显的错误xcode为什么没有挑出来呢？

因为，`createAnimal`方法返回的是`id`类型，编译器无法在编译阶段从`id`推敲出它真正的型别，所以只好先放他过关，然后在执行阶段就crash了。

但是，当你把`id`换成`instancetype`的时候，就像这样：

	@interface Animal : NSObject
	+ (instancetype)createAnimal;
	@end
	
	@implementation Animal
	+ (instancetype)createAnimal
	{
		return [[self alloc]init];
	}	
	@end
	
这样编译器就会在原来crash的地方出现一个红色⚠️了。