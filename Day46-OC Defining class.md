#OC Defining Classes

##基本语法
OC定义一个类的基本语法是这样的：

	@interface SimpleClass : NSObject
		
		
	@end
		
`SimpleClass`继承自NSObject。

##属性(Property)控制着对象的访问

对象经常有一些属性以备外部可以访问。如果你写了一个记录人日常行文的应用，比如，你就需要一个string类型的属性来记录认人的名字。

	@interface Person : NSObject
		
	@property NSString* firstName;
	@property NSString* lastName;
		
	@end
		
在上面这个例子中，`Person`类声明了两个公共属性，它们都是NSString类的实例。

你可能还会增加一个代表人年龄的属性，因为这比只有名字的好得多。你可以用一个number对象代表年龄属性：

	@property NSNumber *yearOfBirth;
	
但是这可能被认为是矫枉过正，我们可以使用C提供的原始类型int来存储年龄这个值：

	@property int yearOfBirth;

##属性标志着数据的访问权限

比如，在`Person`类中，`firstnName`对于一个人来说是不可变的，所以，我们希望这两个属性是可以读取但是不可更改的，如果有人试图更改就会报错，那么，我们就可以这样写：

	@interface Person : NSObject
	
	@property(readonly) NSString *firstName;
	@property(readonly) NSString *lastName;
	
	@end
	
##类中的方法声明

一个参数：

	- (void)someMethodWithValue:(SomeType)value;

多个参数：

	- (void)someMethodWithValue:(SomeType)value1 secondValue:(AnotherType)value2;
	
