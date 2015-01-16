#C++对象内存布局

我们可以用指针的方式取得整个对象的实力内存。因为这些东西在内存中是连续分布的，我们只需要使用适当的地址偏移量，我们就可以获得整个对象的布局。

####单一的一半继承

下面，我们有一个如下的继承关系：

	class Parent{
	public:
		 int parent;
		 Parent():parent(10){}
		 virtual void f(){cout<<"Parent::f()"<<endl;}
		 virtual void g(){cout<<"Parent::g()"<<endl;}
		 virtual void h(){cout<<"Parent::h()"<<endl;}
	};
	
	class Child:pulblic Parent{
	public:
		int ichild;
		Child():ichild(100){}
		vritual void f(){cout<<"Child::f()"<<endl;}
		virtual void g_child(){cout<<"Child::g_child()"<<endl;}
		virtual void h_child(){cout<<"Child::h_child()"<<endl;}
		
	}
	
	class GrandChild:public Child
	{
	public:
		int igrandchild;
		GrandChild():igrandchild(1000){}
		vrital void f(){cout<<"GrandChild::f()"<<endl;}
		vritual void g_child(){cout<<"GrandChild::g_child()"<<endl;}
		vritual void h_grandchild(){cout<<"GrandChild::h_grandchild()"<<endl;}
	}
	
我们使用以下程序作为测试程序：(下面程序中，我们使用了一个int** pVtab来作为遍历对象内存布局的指针，这样，我们就可以方便的像使用数组一样来遍历所有的成员包括其虚函数表了)

	typedf void(*Fun)(void);
	
	GrandChild gc;
	
	int**pVtab=(int**)&gc;
	
	cout<<"[0]GrandChild::_vptr->"<<endl;
	for(int i=0;(Fun)pVtab[0][i]!=NULL;i++)
	{
		pFun=(Fun)pVtab[0][i];
		cout<<"["<<i<<"]";
		pFun();
	}
	
	cout<<"[1]Parent.iparent="<<(int)pVtab[1]<<endl;
	cout<<"[2]Child.ichild="<<(int)pVtab[2]<<endl;
	cout<<"[3]GrandChild.igrandchild="<<(int)pVtab[3]<<endl;
	
其运行结果如下所示：

		[0] GrandChild::_vptr->
    		[0] GrandChild::f()
    		[1] Parent::g()
    		[2] Parent::h()
    		[3] GrandChild::g_child()
    		[4] Child::h1()
   			[5] GrandChild::h_grandchild()
		[1] Parent.iparent = 10
		[2] Child.ichild = 100
		[3] GrandChild.igrandchild = 1000

![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/EntryImages/20081015/dd02.jpg)

可见以下几个方面：

- 虚函数表在最前面的位置。
- 成员变量根据其继承和生命顺序依次放在后面。
- 在单一的继承中，被overwrite的虚函数在心虚函数表中的到了更新。