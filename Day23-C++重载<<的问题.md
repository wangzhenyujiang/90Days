##C++重载<<的问题解决

因为最近想写一个自己的C++的String类，所以用到了好多重载操作符的知识，但是，有一个问题让我的String类进展灰常缓慢，可以说是卡住了，下面就让这个人才(hun dan)隆重登场。

下面上代码：

(首先说明，下面代码语法是正确的，但是运行会报错)：

	class MineString
	{
	friend ostream& operator<<(ostream& out,MineString& string);
	public:
		MineString();
	private:
		int _size;
	}
	
	ostream& operator<<(ostream& out,MineString& string){
		out<<string._size;
		return out;
	}
	
	MineString::MineString():_size(0)
	{
	}
	
	int main(int argc,const char * argv[])
	{
		MineString s;
		cout<<s<<endl;//error
		
		return 0;
	}
	
好的，大家已经赤裸裸(跟我读：guo guo)的看到注释了`error`的那一行，为什么会报错呢？对呀，是吧，嗯。

因为编译器不知道你用的是cout的`<<`还是你自己的`<<`，所以这句话出现了二义性，导致编译错误。嗯就这样！

那有什么解决办法呢，有两种办法：

- 放弃重载<<，而是自己写一个函数用来输出。(妈的，这也叫办法？！)

- 这种方法就比较复杂了，我知道QT中有一个自己的String类，那他是自己又写了一个qDebug类用来输出，当需要输出自己定义的操作符时`qDebud<<str<<endl;`当需要标准库的操作符时`cout<<str<<endl;`。也就是说自己再写一个类似`cout`的类。

