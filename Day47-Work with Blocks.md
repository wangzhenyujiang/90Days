#Work with Blocks

一个Objective-C class 定义一个包含一些组合起来的数据以及相关联行为的对象。

`Blocks`是一个语言水平上在C上的扩展，允许你创建一个代码片段，这些片段可以像值一样传来传去。`Blocks`是Objective-C对象，这意味着它们可以被添加到集合中就像`NSArray`  `NSDictonary`。They also have the ability to capture calues from the enclosing scope,making them simolar to closures or lambdas in other programming languages.

这个章节将会说明声明`Blocks`的语法和回退，展示怎么用`Blocks`来简化比如集合枚举。

##Blocks Syntax

定义一个block的语法是用脱字节(^)，就像这样：

	^{
		NSLog(@"This si a block");	
	}
	
与函数和方法的定义相比，大括号标志着`Blocks`的开始和结束。在这个例子中，block没有返回任何值，也没有接收任何参数。

同样的方法，在C语言中，你可以用一个函数指针指向一个C函数，你可以声明一个变量来指向这个`Block`，就像这样：

	void (^simpleBlock)(void);
	
如果你没有用过C语言的函数指针，这个语法看起来有些困惑。这个例子声明了一个变量叫做`simpleBlock`指向了一个`Block`，这个`Block`没有任何参数也没有返回任何值。这就意味着这个变量可以分配给像上面这样的`Block`，就像这样：

	simpleBlock=^{
	
		NSLog(@"This is a block");
		
	};
	
一旦你声明和关联了一个`Block`变量，你可以这样来调用这个块：

	simpleBlock();
	
##Block Take Arguments and Return Values

`Block`也可以像方法和函数一样传参和返回值。

看一个例子，想一想，一个变量作为一个块的引用，返回两个数字相乘的结果：

	double (^multiplyTwoValues)(double,double);
	
和上面相符合的块是这个样子：

	^(double firstValues,double secondValues){
		return firstValues*secondValues;
	};

一旦你声明和定义了块，你可以想一个函数一样调用它：

	double (^multiplyTwoValues)(double,double)=
	^(double firstValues,double secondValues){
		return firstValue*secondValues;
	};
	
	double result=muliptyTwoValues(2,4);
	
	NSLog(@"The result is %f",result);
	
##Block Can Capture Values from the Enclosing Scope

包含可执行代码，一个块还可以获得一定范围内的状态。

如果你在一个方法中声明了一个块，比如：

	- (void)testMethod
	{
		int anInteger = 42;
		
		void(^testBlock)(void)=^{
			NSLog(@"Integer is %i",anInteger);
		};

		testBlock();
	}
	
在这个例子中，一个整数在块的外部定义了，但是当块被定义的时候这个整数可以被这个块访问。

如果在调用`Block`期间你改变了外部变量的值，就像这样：

	int anInteger = 42;
	
	void (^testBlock)(void)=^{
		NSLog(@"Integer is:%i",anInteger);
	};
	
	anInteger = 84;
	
	testBlock();
	
这个被捕获的值是不受影响的。这意味着日志打印出来：

	Integer is: 42
	
这也意味着块不能改变原始值，或者甚至捕获值(它是以`const`类型的值捕获的)。


