##C++析构函数，复制构造函数,构造函数，重载操作符
看下面这个类：
	
	#include<vector>
	#include<iostream>
	
	class Exml()
	{
		//构造函数
		Exml(){cout<<"Exml()"<<endl;}	
		//复制构造函数
		Exml(const Exml &){cout<<Exml(const Exml &)<<endl;}
		//重载赋值操作符
		Exml & operator = (const Exml &rhe)
		{
			cout<<"Exml & operator = (const Exml &"<<endl;
			return *this;
		}
		//析构函数
		~Exml(){cout<<~Exml()<<endl}
		
	};
	
	void func1(Exml obj)
	{
	}
	
	void func2(Exml & obj)
	{
	}
	
	Exml func3(Exml & obj)
	{
		Exml obj;
		return obj;
	}
	
执行的`main`方法如下：

	int main()
	{
	0	Exml eobj;
	1	func1(eobj);
	2	func2(eobj);
	3	oebj=func3();
	4	Exml *p=new Exml();
	5	vector<Exml> evec(3);
	6   delete p;
		return 0;
	}

运行这段程序后你就会对这几个函数的调用时机和调用顺序非常明白了，下面，就让我给大家讲解一下：

1）第0句，声明了一个`Exml`变量，这个时候就会调用`Exml`类的默认**构造函数**。

2）第1句，将实例`eobj`传给func1函数，大家要注意，func1函数的形参是Exml实例，当调用这个方法的时候，首先会调用**复制构造函数**将`eobj`复制给`obj`，而且，函数执行完毕后，会执行**析构函数**撤销形参Exml对象。

3）第2句，由于func2的形参是一个Exml对象的引用，所以本质上就是将`obj`引用绑定到`eobj`这个实例上，所以不用调用复制构造函数。

4)第3句，首先`func3`调用默认**构造函数**创建局部Exml对象，函数返回时调用**复制构造函数**创建作为返回值副本的Exml对象，然后调用**析构函数**撤销局部Exml对象，然后调用**赋值操作符**，执行完赋值后，调用**析构函数**撤销Exml的副本。

5)第4句，调用**构造函数**动态创建Exml对象，这个对象将在堆上生成。

6)第5句,首先调用**构造函数**创建一个**临时**值Exml对象，然后三次调用**复制构造函数**，将临时对象复制到evec中的每个元素中，然后调用**析构函数**撤销临时Exml对象。

7）第6句，调用**析构函数**撤销动态创建的Exml对象。

最后，当函数`return 0`的时候，evec的生命周期结束，调用**三次析构函数**撤销其中的三个Exml对象。

是不是很有趣呢，嘿嘿。