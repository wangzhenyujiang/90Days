#Work With Protocol

在现实生活中，人们处理一些事物的时候经常要求遵循一些严格的协议。执法官员，例如，需要进行查询和收集证据的时候要“遵循协议”。

在面向对象的编程中，在一个给定的情况下，给这些对象定义一些预期的行为集合是很重要的。比如，一个表视图想要展示一些数据，就要和数据源对象去交流。这就意味着数据源必须响应表视图可能发送的消息集合。

数据源是一个类的实例，就像一个`view controller`或者一个专用数据源类，而且这些类可能集成自`NSObject`。为了知道一个对象是否适合做表视图的数据源，声明这个对象实现必要的方法是非常重要的。

Objective-C允许你定义`protocols`，来声明一个方法以备特殊的情况下可以使用。这个章节描述了定义一个普通协议的语法，而且详细解释一个类怎么遵从一个协议，这就意味着必须实现`required`方法。

##Protocol定义了消息传输合同

一个类声明了与这个类相关的属性和方法。一个协议与之不同的是，用来声明的方法和属性都是独立于任何详细的类。

定义一个协议基本的语法是：

	@protocol ProtocolName
	
	//list of method and properties
	
	@end

协议可以声明的东西既包含实例方法又包含类方法，当然也包括`property`。

看下面一个例子，思考一下，一个自定义的`view`类用来展示一个饼状图。

为了尽可能提高它的可用性，有关信息的所有决定都应该放到另一个数据源对象中。这样就意味着，几个相同的`view`的实例通过不同的数据源就可以展示出不同的信息。

组成一个饼状图视图至少的信息有,几部分组成，每部分的Size，和每部分的Title。饼状图的数据源协议，可能会看起来就像这样：

	@protocol PieCharViewDataSource
	
	- (NSInteger)numberOfSegements;
	- (CGFloat)sizeOfSegmentAtIndex:(NSUInteger)segmentIndex;
	- (NSString *)titleForSegmentAtIndex:(NSUInteger)segmentIndex;
	
	@end

这个饼视图`view`需要一个`property`来跟踪数据源对象。这个对对象可以是任何类，所以这个`property`的属性应该是`id`类型。唯一知道的是事情就是这个数据源对象要实现相关的协议。

在`view`中声明数据源属性的时候，语法应该是这样的：

	@interface PieChartView : UIView
	
	@property (weak) id <PieCharViewDataSource> dataSource;
	
	...
	
	@end
	
OBjective-C用`<...>`来表示复合协议。这个例子声明了一个弱的`property`普通对象指针，而且这个对象实现了`PieChartViewDataSource`协议。

对象是`UIViewController`或者`NSObject`对象并不是很重要。最重要的是它实现了协议，这就意味着`pie chart view`知道了这个数据源对象是可以满足要求的。

##协议可以有可选方法

在某些情况下，在协议声明的所有方法都是`required`的。这意味着，所有实现这个协议的类都必须实现这些方法。

当然，在协议中是有可选方法存在的。这些方法只有一个类需要的时候才实现。

举个例子，你可能认为标题对饼状图来说应该是可有可无的。如果数据源对象没有实现`titleForSegmentAtIndex:`方法，在`view`中将没有标题。

你可以在代码中把可选方法用`@optional`来标记，就像这样：

	@protocol PieChartViewDataSource
	
	- (NSUInteger)numberOfSegment;
	- (CGFloat)sizeOfSegmentAtIndex:(NSUInteger)segmentIndex;
	
	@optional
	- (NSString *)titleForSegmentAtIndex:(NSUInteger)segmentIndex;
	
	@end


在这种情况下，这有`titleForSegmentAtIndex:`方法被标记了可选。在没有被标记前，这个方法时`required`的。

`@iptional`命令对它下面的所有方法都有作用，直到协议结束`@end`或者遇到了其他的命令标记`@required`。你可以加上更多的方法，就像这样：

	@protocol PieChartViewDataSource
	
	- (NSUInteger)numberOfSegments;
	- (CGFloat)sizeOfSegmentAtIndex:(NSUInteger)segmentIndex;
	
	@optional
	- (NSString *)titleForSegmentAtIndex:(NSUInteger)segmentIndex;
	- (BOOL)shouldExplodeSegmentAtIndex:(NSUInteger)segmentAtIndex;
	
	@required
	- (UIColor *)colorForSegmentAtIndex:(NSUInteger)segmentIndex;
	
	@end
	

##在运行时检测optional方法有没有实现

如果一个方法在协议里面被标记为`optional`，当你在试图调用它之前，你必须检查一个对象时候实现了那个方法。

看这个例子，饼状视图`view`必须检测标题方法是否实现了，就像这样：

	NSString *thisSegumentTitle;
	
	if([self.dataSource respondsToSelector:@selector(titleForSegmentAtIndex:)]){
		thisSegmentTitle=[self.dataSource titleForSegmentAtIndex:index];
	}
	
`repsondsToSelector:`方法用了一个selector，指向一个方法编译后的标识符。你通过`@selector`提供给正确的标识符和指定方法的名称。

如果在这个例子中的数据源实现了方法，那么这个title是可用的，如果没有实现这个方法，那么标题就是空。
