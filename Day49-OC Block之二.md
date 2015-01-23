#OC Block之二

##用 _block 变量共享内存

如果你需要用Block改变一个局部变量的值，你可以在开始声明变量的时候用`_block`修饰语。这就意味着，存储的变量是在原始变量的作用范围内和在此范围内声明的任何块之间是共享的。

看下面一个例子，就像之前写的：

	_block int anInteger=42;
	
	void (^testBlock)(void)=^{
		NSLog(@"Integer is:%i",anInteger);
	};
	
	anInteger=84;
	
	testBlock();
	
因为这个`Integer`声明成了`_block`变量，它被声明是为与块共享的。也就是说，这次将会打印出：

	Integer is: 84

这也意味着，这样可以改变原始变量，就像这样：

	_block int anInteger = 42;
	
	void(^testBlock)(void)=^{
		NSLod(@"Integer is: %i",anInteger);
		anInteger=100;
	};
	
	testBlock();
	
	NSLog(@"Value of origin variable is now: %i",anInteger);
	
这次，下面这是输出：

	Integer is: 42;
	Value of original variable is now: 100
	
注语：

添加`_block`修饰符的变量`Block`就可以修改，而非`_block`修饰的变量就不可以修改。里面的实现对我来说很复杂，唐巧的《iOS进阶》中讲的很清楚(当然我没看明白)。但是唐巧在书中用了两句话总结，我就写在这里了：

**对于block外的变量引用，block默认是将其复制到其数据结构中来实现的访问。如果这个对象是一个引用类型，则block会将其引用计数加1.**

**对于用_block修饰的外部变量引用，block是复制其引用地址来实现访问的。**

这样一说，是不是心里就很清晰了呢？嘿嘿，Nice!
	

##你可以把块当做方法或者函数的参数

在前面的每个例子中，我们都是定义好块之后就直接调用。块作为参数在函数和方法之间传输是很平常的事情。你可以用`Grand Central Dispatch`在后台调用块，比如，定义一个块去不停地执行一个任务，就像当列举一个集合。并发性和枚举将在后面会说到。

块也用来作回调函数，当一个方法执行完后会回调这个定义的代码块。举个例子：你的app应该需要响应用户的某个请求，你就会创造一个对象完成一个任务，就像从一个web服务器请求一些数据。因为这个任务可能会花费很长时间，当任务在执行的时候应该展示一个等待指示器，一旦任务完成就要隐藏掉这个指示器。

你可能会用委托来完成这个任务：你需要定义一个恰当的委托协议，实现必须要的实现的方法，设置对象的任务委托，这样，一旦任务完成就可以调用协议方法。

块使这种情况变得简单，因为你可以定义一个回调行为当你完成任务的时候，就像这样：

	- (IBAction)fetchRemoteInfomation:(id)sender
	{
		[self showProgressIndicator];
		
		XYZWebTask *task = ...
		
		[task beginTaskWithCallbackBlock:^{
			[self hideProgressInficator];
		}];
	}

这个方法调用了一个方法去展示等待指示器，然后启动一个耗时的任务。回调块定义了当任务完成后要做的事情；在这种情况下，它很简单的调用了一个方法来隐藏等待指示器。**注意，这个回调块获取了self引用，用来调用hideProgressIndicator函数**。关心获取self这种情况是很重要的，因为这很容易产生一个强引用环。这种情况将在后面介绍。


在代码的可读性上，使用块也比协议有着巨大的优势。

`beginTaskWithCallbackBlock`函数的声明应该是这样的：

	- (void)beginTaskWithCallbackBlock:(void(^)(void))callbackBlock;

`(void(^)(void))`表明了参数是一个没有任何输入和返回值的块。在这个函数的实现中，我们可以调用这个传进来的块：

	- (void)beginTaskWithCallbackBlock:(void(^)(void))callbackBlock{
		...
		
		callbackBlock();
	}
	
如果方法的参数是有多个参数和返回值的块，那也是一样的：

	- (void)doSomethingWithBlock:(void^(double,double))block{
		...
		
		block(2.0,2.0);
	}
	
##块应该总是方法的最后一个参数

如果一个方法只需要一个块作为参数那是最好的，但是，如果一个方法还需要不是块的参数的时候，块应该放到最后：

	- (void)beginTaskWithName:(NSString *)name completion:(void(^)(void)callback;

这个格式的方法当内联的展开的时候是很方便读的：

	[self beginTaskWithName:@"MyTask" completion:^{
		NSLog(@"the task is complete");
	}];
	

