#小解OC Categories

`Categories`是Objective-C最有用的一个特性。事实上，`Category`可以使你为一个类添加一个方法，而不需要继承这个类甚至不用知道这个类的内部实现的细节。

这是非常有用的因为你可以为内建的对象添加方法。如果你想把在你应用中所有的的NSString实例添加一个方法，那你只加一个`Category`就好了。并不是所有的事情都需要自定义子类的。

比如，如果你想给NSString添加一个方法来判断字符串本身是否是URL，那就会像下面这样：

	#import <Cocoa/Cocoa.h>
	
	@interface NSString (Utilities)
	- (BOOL)isURL;
	
	@end
	
这很像声明一个类。他们之间的区别是没有超类列表，而且在圆括号中有一个`Category`的名字。这个名字可以是任何你想要的，当然它应该尽量表达出新添加的方法做的是什么。

下面是implementation的代码：

	#import "NSString-Utilities.h"
	
	@implementstion NSString (Utilities)
	
	- (BOOL)isURL
	{
		if([self hasPrefix:@"http://"])
		{
			return YES;
		}else
		{
			return NO;
		}
	}
	
	@end
	
现在任何NSString对象都可以用`isURL`这个方法。看下面的测试代码：

	NSString* string1=@"http://www.google.com";
	NSString* string2=@"google";
	
	if([string1 isURL])
		NSLog(@"string1 is a URL");
	
	if([string2 isURL])
		NSLog(@"string2 is a URL");

不像继承，分类不能添加实例变量。当然，你可以用分类技术来重写在类中已经存在的方法，但是你应该非常小心。

记住，当你对一个类使用`Category`技术的时候，它对你应用中所有这个类的实例都有影响。