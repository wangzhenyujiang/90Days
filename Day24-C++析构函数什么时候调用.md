##C++析构函数什么时候调用

**语言，一遍一遍的学，一遍一遍的看，才能品味到..它的美！**(屁话这么多，你不就是忘了它的用法回头看看嘛，说的这么高大上)人艰不拆好吧各位！

####前言：
析构函数在什么时候会被调用，在什么时候需要手动来调用，今日在此一辩！

####正文

对象在构造的时候系统会分配内存资源，对一些数据成员进行初始化或者赋值；一个良好的class需要有资源回收机制，而这一操作便落在了析构函数上，析构函数来负责类内的资源的free。来看一段代码：

	calss MineString
	{
	public:
		MineString(const char* str);
		vrital ~MineString();
	private:
		char *_string;
		int _size;
	};
.cpp中的实现代码：

	MineString::MineString(const char* str)
	{
		_size=strlen(str);
		_string=new char[_size+1];//here
		strcpy(_string,str);
		_string[_size]=NULL;
		cout<<"构造函数"<<endl;
	}
	MineString::~MineString()   //析构函数实现
	{
		if(_string)
		{
			delete _string;     //释放内存
		}
		cout<<"析构函数"<<endl;
	}
	
如果你在main函数中这样写：

	int main()
	{
		MineString("Hello");
		return 0;
	}
你会得到这样的执行结果：

	构造函数
	析构函数
	请按任意键继续...
	
也就说在main函数中如果直接声明一个对象，在声明的时候会直接调用类内的构造函数，在主函数结束之前的那一刻，也会自动调用这个类的析构函数。

再看下面的代码：

	int main()
	{
		MineString * string;
		return 0;
	}
	
执行结果：

	请按任意键继续...
在main函数中，如果直接声明一个对象指针(只是声明),即不自动调用构造函数也不调用析构函数。

但是将代码改成如下：

	int main()
	{
		MineString *string;
		string = new MineString("Hello");
		return 0;
	}
	
执行结果：

	构造函数
	请按任意键继续...
对一个指针分配一个内存空间，调用了构造函数，但是并没有调用析构函数。因为C++缺少这一机制。需要你手动的delete。

	int main()
	{
		MineString * string;
		string=new MineString("Hello");
		delete string;
		return 0;
	}

执行结果：

	构造函数
	析构函数
	请按任意键继续...
	

