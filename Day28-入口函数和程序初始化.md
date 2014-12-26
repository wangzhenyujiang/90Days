#入口函数和程序初始化1

一直对一个问题很感兴趣，`就是程序从main函数开始吗？`

####程序从main函数开始吗

首先来看几个证据：

######Number ONE

	#include <stdio.h>
	#include <stdlib.h>
	
	int a=3;
	
	int main(int argc,char* argv[])
	{
		int *p=(int *)malloc(sizeof(int));
		scanf("%d",p);
		println("%d",a+*p);
		free(p);
	}
	
从代码中我们可以看出，在程序刚刚执行到`main`的时候，**全局变量的初始化过程已经结束了(a的值已经确定了)，**main函数的两个参数(`argc`和`argv`)也被正确传进来了。此外，在你不知道的时候，堆和栈的初始化已经悄然完成了，一系列I/O也被初始化了，因此可以放心的使用`printf`和`malloc`。

######Number TWO

在C++中main函数之前执行的代码还会更多，例如代码如下：

	#include<iostream>
	using namespace std;
	string v;
	double foo()
	{
		return 1.0;
	}
	double g=foo();
	
	int main()
	{}
在这里，对象v的构造函数，以及用于初始化全局变量g的函数`foo`都会在`main`函数之前调用。

######Number THREE

在C和C++中有一个函数特别好玩：`atexit`，这个函数接受一个函数指针作为参数，并保证在函数正常退出(指从main里返回或调用exit函数)时，这个函数指针指向的函数会被调用。

	void foo()
	{
		printf("bye!\n");
	}
	int main()
	{
		atexit(&foo);
		printf("end of main!\n");
		return 0;
	}
	
执行完的结果是：

	end of main!
	bye!
	


操作系统装载程序之后，首先运行的代码并不是main函数的第一行，而是某些别的代码，这些代码负责**准备好main函数执行所需要的环境，并且负责调用main函数。**这时候你才可以在main函数中放心大胆的鞋各种代码：申请内存，使用系统调用等，它会记录main函数的返回值，调用atexit注册的函数，然后结束进程。

运行这些代码的函数称为入口函数或入口点，他们往往是运行库的一部分。