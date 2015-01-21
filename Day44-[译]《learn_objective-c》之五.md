#[译]《Learn Objective-C》之五

##Designing a Class Interface

`Objective-C`创建一个`Class`的语法是很简单的。它一贯被分为两部分。

`class interface`经常被存储在`.h`文件中，和一些定义的实例变量和公共方法。

实现是`.m`文件而且包含了头文件中方法实现的代码，在这里也总是定义一些私有的方法，这些方法对客户端是不可见的。

下面这个类叫做`Photo`，所以这个文件叫做`Photo.h`:

	#import <Cocoa/Cocoa.h>
	
	@interface Photo:NSObject
	{
		NSString* caption;
		NSString* photographer;
	}
	@end
第一，我们import Cocoa.h，来引进所有基础的类库。`#import`命令可以自动的防止引进一个文件多次。

####Add Methods

让我们为这些实例变量来添加一些`getters`。

	import <Cocoa/Cocoa.h>
	
	@interface Photo:NSObject
	{
		NSString* caption;
		NSString* phptographer;
	}
	
	- caption;
	- photohrapher;
	
	@end;
	
记住，Objective-C方法习惯性的忽略`get`前缀。在方法前一个`-`意味着这个方法是一个实例方法。一个`+`在方法前意味着这个方法是个类方法。

默认情况下，编译器允许一个方法返回一个`id`类型的对象，而且，参数都是`id`类型。以上的代码严格意义上来说是正确的，但是平常情况下我们不是这么写的。让我们给这些方法加上些详细的返回类型。

	#import <Cocoa/Cocoa.h>
	
	@interface Photo:NSObject
	{
		NSString* caption;
		NSString* photographer;
	}
	
	- (NSString*)caption;
	- (NSString*)phptographer;
	
	@end
现在，让我们加上`setter`方法：

	#import <Cocoa/Cocoa.h>
	
	@interface Photo:NSObject
	{
		NSString* caption;
		NSString* photorapher;
	}	
	
	- (NSString*)caption;
	- (NSString*)photographer;
	
	- (void)setCaption:(NSString*)input;
	- (void)setPhotographer:(NSString*)input;
	
	@end
	
`setter`不需要返回任何值，所以是`void`类型。
