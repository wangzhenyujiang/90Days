#IOS开发 Protocol协议及委托代理(delegate)传值

####前言
Protocol(协议)只能定义公用的一套接口，但不能提供具体的实现方法。也就是说，它只告诉你要做什么，但是具体怎么做它不关心。

####Protocol(协议)的作用

一、定义一套公用的接口(Public)

- @required:必须实现的方法
- @optional:可选实现的方法(可以全部不实现)

二、委托代理(Delegate)传值

**意思是委托别人去做某事。**

比如：两个类之间的传值，类A调用类B的方法，类B在执行过程中遇到问题通知类A，这时候我们需要用到代理(Delegate)。
再比如：控制器(Controller)与控制器(Controller)之间的传值，从C1跳转到C2，再从C2返回到C1时需要通知C1更新UI或者是做其他的事情，这时候，我们就用到了代理(Delegate)传值！

##定义一套公用接口(public)

- 新建一个协议文件，文件类型选择(Protocol)。

ProtocolDelegate.h代码(协议不会生成.m文件)：

	@protocol ProtocolDlegate <NSObject>
	
	//必要实现的方法
	@required
	-(void)error;
	
	//可选实现的方法
	@optional
	-(void)other;
	-(void)other2;
	-(void)other3;
	
	@end
	
需要使用到协议的类，import它的头文件：

	#import "ViewController.h"
	#import "ProtocolDelegate"

要遵守协议：

	@interface ViewController()<ProtocolDelegate>
	
	@end
在.m文件中，要实现必须实现的方法：

	-(void)error
	{
	
	}
	
##委托代理(Delegate)传值

在两个Controller之间实现传值。定义两个ViewController,一个命名为ViewController另一个命名为ViewControllerB.

下面是ViewControllerB.h的部分代码：

	//新建一个协议，协议的名字一般是由“类名+Delegate”
	@protocol ViewControllerBDelegate <NSObject>
	
	//代理传值方法
	-(void)sendValue:(NSString *)value;
	
	@end
	
ViewControllerB.m文件的部分代码：

	-(IBAction)backAction:(id)sender
	{
		if([_delegate respondsToSelector:@selector(sendValue:)])
		{
			[_delegate sendValue:_textFiled.text];
		}
		[self.navigationController popViewControllerAnimated:YES];
	}
	
在第一个ViewController中，要实现ViewControllerBDelegate协议,并且要实现sendValue方法：

	@interface ViewController()<ViewControllerBDelegate>
	@end
	
	@implementation ViewController
	-(void)sendValue:(NSString *)value
	{
		UIAlterView *alterView=[[UIAlterView alloc]initWithTitle:@"成功" message:value delegate:nil cancelButton:@"确定" otherButtonTitles:nil,nil];
		[alterView show];
	}
	
	@end
	
	
[Preference](http://www.imooc.com/wenda/detail/243235)

