#C++操作符重载

####算数操作符+重载

**一般而言，将算数操作符和关系操作符定义为非成员函数，像下面给出的Sales_item加法操作符一样。**

	Sale_item operator+(const Sale_item& lhs,const Sale_item& rhs)
	{
		Sale_item ret(lhs);
	    //copy lhs into a local object that we'll return 
		ret += rhs;
		return ret;
	}
	
加法操作符并不改变操作数的状态，操作数是对const类型对象的引用；相反，它将产生并返回一个新的Sale_item对象，该对象初始化为lhs的副本，并用复合赋值操作符(+=)来加入rhs的值。

**算数操作符通常产生一个新值，该值是两个操作数的计算结果，它并不同于任操作数且在一个局部变量中计算，返回对那个变量的引用是一个运行时错误**

即定义了算数操作符又定义了相关复合赋值操作符的类，一般应使用复合赋值实现算数操作符。

####赋值操作符=重载

**赋值操作符重载函数必须是类成员函数**

类赋值操作符接受类类型的参数，通常该类型是对类类型的const引用，但也可以是类类型或对类类型的非const引用。如果没有定义这个操作符，则编译器将合成它。类赋值操作符必须是类的成员，以便编译器可以知道是否需要合成一个。

可以为一个类定义很多附加的操作符，这些赋值操作符会因右操作数的不同而不同。

比如String类包含如下的成员:

	class String
	{
	public:
		//...
		String& operator=(const String&);
		String& operator=(const char *);
		String& operator=(char);
		//...
	};

**赋值必须返回对*this的引用**

返回值返回的通常是左操作数的引用：

	Sale_item& Sale_item::operator+=(const Sale_item rhs)
	{
		units_sold+=rhs.units_sold;
		revenue+=rhs.revenue;
		return *this;
	}

一般而言，赋值操作符和复合赋值操作符一般会返回左操作数的引用。