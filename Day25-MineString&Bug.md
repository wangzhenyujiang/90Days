#MineString项目中遇到的bug
有bug不是坏事，但是有解决不了的就不怎么地了，但是慢慢的解决小bug真的很让人开心，嘿嘿。

####今天犯了删除空指针的错误

重载=操作符的时候我是这么写的：

	MineString& MineString::operator=(MineString& str)
	{
		if(_size!=0)
		{
			_size=str._size;
			_string=str._string;
		}
	}
	
下面是析构函数：

	MineString::~MineString()
	{
		if(_string)
		{
			delete _string;
		}
	}
下面是main函数：

	int main()
	{
		MineString str("Hello");
		MineString str2="你好";
		str2=str;
		str2.MinePrint();
		
		return 0;
	}
	
小伙伴们，看到错了么，嘿嘿，虽然是个小错，但是没有强大的C++基础韩式不容易看出来的，我也是各种设置断点发现的。

大家看程序，在重载=的时候，我让this->_string直接指向了传入形参的str.string。也就是说，在main中，运行了str2=str后，str2._string和str._string指向了同一块内存。但是问题来了，当程序结束的时候，要分别调用析构函数收回这两个对象，当然，调用析构函数的顺序是按着对象的声明顺序相反的方向来的。所以调用str2的析构函数的时候，对身它们共同指向的内存执行delete操作，没错，但是回收str的时候又对它们共同指向的内存执行了一下delete，因为之前已经释放掉了，再次释放就会报错了。

解决的方法是，重载=操作符写成强拷贝：

	MineString& MineString::operator=(const MineString& str)
	{
		if(_size!=0)
		{
			_size=str._size;
			delete [] _string;
			_string=new char[_size+1];
			strcpy(_string,str._string);
		}
	}