#[译]Chapter3 You May Not Know...

##最好的命名实践
通览iOS，命名规范是非常重要的。在下面的几个章节中，你会学到怎么正确的命名变量和以及这样做的重要性。

####Automatic Variables

Cocoa是一种动态类型的语言，你会很容易的对自己使用的类型感到困惑。集合(数组，字典，等等)没有与之相关的类型，所以，你可以很简单的写出向下面这样的代码：

	NSArray *dates=@[@"1/1/2000"];
	NSDate *firstDate=[dates firstObject];
	
这些代码会无误的编译，但是当你试着用`firstDate`，应用会崩溃掉因为一个未知的异常。这个错误是调用一个string类型的数组`dates`。这个数组本应该被命名为`dateStrings`，或者它应该包含一个NSDate对象。这种良好的命名习惯会让你减少很多头疼。


#Methods

方法的名字应该使它的参数和返回类型非常清晰。比如：
下面这个方法的名字就非常令人困惑：

	- (void)add;//Confusing
看上去`add`应该有一个参数，但是它没有。难道它加的是默认对象？

再来看下面这些清晰的名字：

	- (void)addEmptyRecord;
	- (void)addRecord:(Record *)record;
	
现在，`addRecord`方法非常清晰：接收一个`Record`参数。对象的类型应该和名字匹配，这样就不会产生疑惑了。比如，这里有一个例子显示了平常的一个错误：

	- (void)setURL:(NSString*)URL;//Incorrect
	
这是不正确的因为这个方法叫做`setURL`，应该接收一个NSURL，而不是一个NSString。如果你需要一个string，你需要加上一些指示让方法变得清晰些：

	- (void)setURLString:(NSString *)string;
	- (void)setURL:(NSURL *)URL;

这个规则不应该被过度的使用。如果变量的类型很明显就不要把类型信息追加到变量后面。有一个属性叫做`name`总比叫做`nameString`好。

方法也有一些详细的规则关系到内存管理。尽管ARC使一些规则变得不怎么重要，但是不正确的命名规则会导致很难解决的bug当ARC和非ARC代码在一起混用的时候。

一些以`get`开始的方法应该返回一个值的引用。比如：

	- (void)getPerson:(Person **)person;
	
不要用前缀`get`作为属性获取器的一部分。属性获取器的名字应该是变量名字本身。

