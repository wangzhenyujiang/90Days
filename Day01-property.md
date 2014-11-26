#理解“属性-property”这一概念

>研究OC的**属性**这一抽象的概念也有些日子了，但是自己写代码的时候还是不是很明白，今天带着大家看看Effective Objective-C对这一概念是怎么描述的吧。

用通俗的话说“属性”的意思是：编译器会自动写出一套存取方法，用已访问给定类型中具有给定名称的变量。
比如这个类：

	@interface Person:NSObject
	@property NSString *firstName;
	@property NSString *lastName;
	@end
	
对于该类的使用者来说，上书代码写出来的类与下面这种写法等效：
	
	@interface Person:NSObject
	-(NSString*)fristName;
	-(void)setFristName:(NSString*)fristName;
	-(NSString*)lastName;
	-(void)setLastName:(NSString*)lastName;
	@end
	
要访问属性，使用“点语法”和直接调用存取方法的效果相同：
	
	Person *aPerson = [Person new];
	
	aPerson.firstName=@"Bob";//same as
	[aPerson setFirstName:@"Bob"];
	
	NSString *lastName=aPerson.lastName;//same as
	NSString *lastName=[aPerson lastName];
	
如果使用了属性的话，那么编译器就会自动编写访问这些属性所需要的方法。这个过程在**编译时**执行，编辑器里看不到这些“合成方法”的源代码。除了生成方法之外，编译器还要自动向类中添加适当类型的实例变量，并且在属性名前面加**下划线**，以此作为实例变量的名字。也可以在类的实现代码中通过`@synthesize`语法来指定实例变量的名字：
	
	@implementation Person
	@synthesize firstName = _myFirstName;
	@synthesize lastName = _myLastName;
	@end
	
这样的话就不再使用默认的名字了，建议不要这么做。
##属性特质


####原子性
默认情况下，编译器所合成的方法会通过锁定机制确保其原子性。如果属性具备`nonatomic`特质，则不适用同步锁。开发iOS程序是应该使用`nonatomic`属性，因为`atomic`属性会严重影响性能。
####内存管理语义
>在Effective Objective-C中，这部分写的有点晦涩难懂，可能是我境界没达到吧。在此献丑了：

**readwirte**:是可读可写属性，需要生成`getter`和`setter`方法；

**readonly**:是只读属性，只会生成`getter`方法。

**assign**:是赋值特性，`setter`方法将传入参数赋值给实例变量（把某块内  存比作一个门，只有一把钥匙，变量们同进同出，钥匙丢了，就全进不去了），相于于指针赋值，不对引用计数进行操作，注意原对象不用了，一定要把这个设置为nil，用于基础数据；

**retain**:表示持有特性，`setter`方法将传入参数先保留，后赋值（两把钥匙，各自进出），传入参数的`retaincount`加1，相当于对原对象的引用计数加1.

当然，自从ARC引进之后，`assign`和`retain`就被`strong`和`weak`代替，而`weak`比`assgin`多了一个功能，就是对象消失后把指针设置为`nil`，避免了野指针的出现。`strong`等同于`reatin`。


最后解决`copy`这个让我疑惑很久的东西，`copy`是一个对象变成新的对象(新内存地址) 引用计数为1，原来对象计数不变，也就是说他自己开辟了一块新的内存而且引用计数是1，那么问题又来了，为什么NSString总是用`copy`呢？

因为：**NSString 为 NSMutableString 的基类，如果将NSMutableString 以retain的形式赋值给NSString后，后续修改NSMutableString会导致NSString内容的变化，这通常不是我们希望的，所以用copy最安全**。也就是说，你把A赋给了B，想改A又不想改B，那就让B自己另起家业就行了，给B点钱让他滚蛋，以后老子怎么变你也管不着了。


>我靠，最后这一坨都写成翔了，不过是自己的一点点小理解，以后会整理好思路再动键盘，明天会研究一下内存管理的问题。欢迎喷我T_T(和交流)。