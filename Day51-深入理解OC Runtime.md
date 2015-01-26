#Runtime TWO

##先看几个概念

####什么是id

id在objc.h中定义如下：

	//A pointer to an instance of a class.
	typedef struct objc_object *id;

就像注释中所说的这样id是指向一个objc_object结构体的指针。

####objc_object

objc_object在objc.h中的定义：

	//Represents an instance of a class.
	struct objc_object
	{
		Class isa;
	}
	
这个时候我们知道OBjective-C中的object在最后会被转换成C的结构体，而在这个struct中有一个isa指针，指向它的类别Class(C是大写的)。

####Class(C大写的)

在objc.h中定义如下：

	//An opaque type that represents an Objective-C class
	typedef struct objc_class *Class;
	
我么可以看到，Class本身也是指向一个C的struct`objc_class`。

	struct objc_class
	{
		Class isa OBJC_ISA_AVAILABILITY;
		
		Class super_class;
		const char *name;
		long version;
		long info;
		long instance_size;
		struct objc_ivar_list *ivars;
		struct objc_method_list **methodLists;
		struct objc_cache *cache;
		struct objc_protocol_list *protocols;	
	};

该结构中，isa指向所属的Class,super_class指向父类别。

在`objc_runtime_new.h`中，我们发现`objc_class`有如下定义：

	struct object_class : objc_object
	{
		//Class ISA
		Class superclass;
		
		...
		...
	}
	
在看到Objctive-C的设计哲学中，一切都是对象。这个Class对应的类我们叫做`Meta Class`。即Class结构体中的isa指针指向的就是它的`Meta Class`。

##Meta Class

根据上面的描述，我们可以把`Meta Class`理解为`一个Class对象的class`。简单的说：

- 当我们发送一个消息给一个NSObject对象时，遮天消息会在对象类的方法列表里查找。
- 当我们发送一个消息给一个类时，这条消息会在类的Meta Class的方法列表中查找

而Meta Class也是一个Class，它跟其他Class一样有自己的isa和super_class指针。

具体的如图所示：
![](http://106.186.113.24:8888/other/Class%26MetaClass.001.jpg)

- 每个Class都有一个isa指针指向一个唯一的Meta Class
- 每一个Meta Class的isa指针都指向嘴上层的Meta Class
- 最上层的Meta Class的super class指向自己，形成一个环路
- 每一个Meta Class的super class指针指向它原本Class的 Super Class的Meta Class。但是最上层的Meta Class的 Super Class指向NSObject Class本身
- 最上层的NSObject Class的super Class指向nil





