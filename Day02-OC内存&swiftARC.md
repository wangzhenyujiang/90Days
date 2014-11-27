#没有ARC陪伴的OC和swift的内存管理

>尽管`Apple`已经给出了ARC机制，但是明白`ARC`的原理和没有产生`ARC`机制之前内存管理是怎么运作的还是很有用的！

##没有ARC陪伴的Objective-C内存管理

>Objective-C的内存管理机制与.Net/Java那种全自动的垃圾回收机制是不同的，它本质上还是C语言中得手动管理方式，只不过稍加了一些自动方法。


1 .Objective-C的对象生成在堆上，生成之后，需要一个指针来直向它。  

	ClassA *obj1 =[[ClassA alloc]init];
	
2 .Objective-C的对象在使用完成之后不会自动销毁，需要执行dealloc来释放空间（销毁），否则内存泄露。

	[obj1 dealloc];
	
那么，下面代码中obj2是否需要调用dealloc？

	Class *obj1=[[ClassA alloc]init];
	Class *obj2=obj1;
	[obj1 hello];//输出Hello
	[obj1 dealloc];
	[obj2 hello];//能够执行这一行和下一行吗？
	[obj2 dealloc];
不能，因为obj1和obj2只是指针，他们指向同一个对象，[obj1 dealloc]已经销毁这个对象了，不能再调用`[obj2 hello]`和`[obj2 dealloc]`。obj2实际上是一个无效指针。

3 .Objective-C采用了引用计数。对象内部保存一个数字，表示被引用的次数。例如，某个对象被两个指针所指向（引用）那么它的`retain count`为2.需要销毁对象的时候不直接调用`dealloc`，而是调用`realease`。`realease`会让`retain count`减1，只有`retain count`等于0，系统才会调用`dealloc`真正的销毁这个对象。

	ClassA *obj1=[[ClassA alloc]init];//对象生成，retain count=1
	[obj1 release];//release使retain count减1，retain count=0，dealloc自动被系统调用，对象被销毁
	
4 .Objective-C指针赋值时，retain count不会自动增加，需要手动retain。

	[Class obj1]=[[ClassA alloc]init];//ratain count =1
	[Class obj1]=obj1;//retain count =1
	[obj2 retain];//retain count =2
	[obj1 hello];//输出hello
	[obj1 release];//retain count =2-1=1
	[obj2 hello];//输出hello
	[obj2 release];//retain count =1-1=0,对象被销毁
	
**注意：** 如果没有调用`[obj2 release]`,这个对象的`retain count`始终是1，不会被销毁，内存泄露。

5 .后来,`Apple`又引入了` autorelease pool`(自动释放对象池)，它的原理很简单:

- `NSAutoreleasePool`内部包含一个数组（NSMutableArray），用来保存声明为autorelease的所有对象。如果一个对象声明为`autorelease`，系统所做的工作就是把这个对象加入到这个数组中去。
- `NSAutoreleasePool`自身在销毁的时候，会遍历一遍这个数组，`release`数组中的每个成员。如果此时数组中成员的`retain count`为1，那么`release`之后，`retain count`为0，对象正式被销毁。如果此时数组中成员的`retain count`大于1，那么`release`之后，`retain count`大于0，此对象依然没有被销毁，内存泄露。

>好了，以上就是在没有ARC机制之前的内存管理工作原理，还挺好玩的，自己感觉ARC机制实际上就是C++中的智能指针。

##ARC下的swift

>在swift中，ARC机制和OC2.0中的ARC很相似，特别是那些property属行基本没有改变。这里说一下swift中的实例间的强引用环问题。

*`swift`解决这个问题有两种方法，一个是**弱引用**一个是**无主引用*** 这里先介绍一下**弱引用**

弱引用不会增加实例的引用计数，因此不会阻止ARC销毁被引用的实例。这种特性使得引用不会变成强引用环。声明属性或者变量的时候，关键字weak表明引用为弱引用。

>注意弱引用只能声明为变量类型，因为运行时它的值可能改变。弱引用绝对不能声明为常量。

因为弱引用可以没有值，所以声明弱引用的时候必须是可选类型的。在Swift语言中，推荐用可选类型来作为可能没有值的引用的类型。

如前所述，弱引用不会保持实例，因此即使实例的弱引用依然存在，ARC也有可能会销毁实例，并将弱引用赋值为nil。你可以想检查其他的可选值一样检查弱引用是否存在，永远也不会碰到引用了也被销毁的实例的情况。

	class Person {
         let name: String
         init(name: String) { self.name = name }
         var apartment: Apartment?
         deinit { println("\(name) is being deinitialized") 
    }
    calss Apartment{
         let number:Int
         init(number:Int){self.number=number}
         weak var tenant:Person?
         deinit{println("Apartment\(number)is being deinitialized")}
    }
    
         var john:Person?
         var number73:Apartment?
    
         john =Person(name:"John Appleseed")
         number73=Apartment(number:73)
    
         john!.apartment=number73
         number73!.tenant=john
         
Person的实例仍然是Apartment实例的强引用，但是Apartment实例则是Person实例的弱引用。这意味着当破坏john变量所持有的强引用后，不再存在任何Person实例的强引用.
既然不存在Person实例的强引用，那么该实例就会被销毁：

	john = nil 
	//打印"John Appleseed is being deinitialized"
	
只有number73还持有Apartment实例的强引用。如果你破坏这个强引用，那么也不存在Apartment实例的任何强引用.

这时，Apartment实例也被销毁:

	number73 = nil
	// 打印"Apartment #73 is being deinitialized" 

上面的两段代码表明在john和number73赋值为nil后，Person和Apartment实例的deinitializer都打印了“销毁”的消息。这证明了引用环已经被打破了。

**References:**
[Initialization](https://developer.apple.com/library/ios/documentation/swift/conceptual/swift_programming_language/AutomaticReferenceCounting.html)







