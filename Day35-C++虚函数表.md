#C++虚函数表

####前言
对C++了解的人都知道虚函数(Virtual Funcion)是通过一张虚函数表(V Table)来实现的。简称V-Table。在这个表中，主要是一个类的虚函数的地址表，这张表解决了继承、覆盖的问题。这样，**在有虚函数类的实例中，这个表被分配在了这个实例的内存中**，所以，当我们用父类的指针来操作一个子类的时候，这张虚函数表就显得尤为重要了，它就像一个地图一样，指明了实际应该调用的函数。

C++的编译器应该是保证虚函数表的指针存在于对象实例中**最前面的位置**(这是为了保证取到虚函数表的有最高的性能--如果有多层集成或是多重继承的情况下。)这意味着我们通过对象实例的地址得到这张虚函数表，然后就可以遍历其中函数指针，并调用相应的函数。

####虚函数表

下面看一个例子：

	class Base{
		public:
			virtual void f(){cout<<"f()"<<endl;};
			virtual void g(){cout<<"g()"<<endl;};
			virtual void h(){cout<<"h()"<<endl;};
	};
按照上面的说法，我们可以通过Base的实例来得到虚函数表：

	int main()
	{	
	Base b;
	
	cout<<"虚函数表地址："<<(int *)(&b)<<endl;
	cout<<"虚函数表-第一个函数地址："<<(int *)*(int *)(&b)<<endl;
	return 0;
	}
实际运行效果如下(MacOS10.10  G++4.2.1)：

	虚函数表地址：0x7fff52a4fc20
	虚函数表第一个函数的地址：0xd1b20f0
	

通过这个栗子，我们可以看到，通过强行把`&b`转成`int*`,取得虚函数表的地址。

其实还可以得到虚函数的具体的地址的，取得虚函数表的地址，然后再次取地址就可以的到第一个虚函数的地址了。

####一般继承(无虚函数覆盖)

下面，让我们看看集成时的虚函数表是什么样子的。假设有如下所示的一个继承关系:

	class Base
	{
	public:
		virtual void f(){};
		virtual void g(){};
		virtual void h(){};
	};
	
	class Derive:public Base
	{
	public:
		virtual void f1(){};
		virtual vo，id g1(){};
		virtual void h1(){};
	};
注意，在这个继承关系中，子类没有重载任何父类的函数。子类的虚函数表会是这样的：

![子类虚函数表](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_vtable2.JPG)
我们可以在表中看到如下几点：

- 虚函数按照其声明顺序放于表中。
- 父类的虚函数在子类虚函数的前面。

大家可以编一段程序来验证一下。

####一般继承(有虚函数覆盖)
下面栗子：

	class Base
	{
	public:
		virtual void f(){};
		virtual void g(){};
		virtual void h(){};
	};
	
	class Derive:public Base
	{
	public:
		virtual void f(){};
		virtual vo，id g1(){};
		virtual void h1(){};
	};
注意，子类覆盖了父类的一个虚函数`f()`。那么子类的虚函数列表会是什么样子的呢？

![有虚函数覆盖](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_vtable3.JPG)

- 覆盖的`f()`函数被放到了虚函数表中原来父类虚函数的位置。
- 没有被覆盖的函数依旧。

于是就有了下面这一段代码：

	Base *p=new Derive();
	b->f();
由`b`所指向的内存中的虚函数表`f()`的位置已经被Derive::f()函数地址取代，于是在实际调用发生时，是Derive::f()被调用了。这就实现了多态。

####结语
大家最好都用代码实现以下，嗯，就是这样的。

####Perfence

[无名英雄](http://blog.csdn.net/haoel/article/details/1948051)


