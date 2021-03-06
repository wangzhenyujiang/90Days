#G++&cpp==ubuntu

####C++编程中相关文件后缀

- `.a` 静态库
- `.h` C或者C++源代码头文件
- `.ii` C++源代码(不需要编译预处理)
- `.o` 对象文件
- `.s` 汇编语言代码
- `.so` 动态库

####单个源文件生成可执行程序

下面是一个保存在文件helloworld.cpp中的一个简单的C++程序的代码：

	#include <iostream>
	int main(int argc,char *argv[])
	{
		std::cout<<"HelloWorld"<<endl;
		return 0;
	}
程序很简单，输出一个HelloWorld，该代码可用以下命令编译为可执行文件：

	g++ helloworld.cpp
编译器g++通过检查命令行中指定的文件的后缀名可识别其位C++源代码文件。编译器默认的动作：**编译源代码文件生成对象文件（object file），链接对象文件和libstdc++库中的函数得到可执行程序。然后删除对象文件。由于命令行中未指定可执行程序文件名，编译器采用默认的a.out。**

程序运行：

	./a.out
更为普遍的是用`-o`选项指定可执行程序的文件名：

	g++ helloworld.cpp -o helloworld
在命令行中输入程序名可以使之运行：

	./helloworld
####多个源文件生成可执行程序

假如有以下文件：

	/*speak.h*/
	#include<iostream>
	class Speak
	{
		public:
			void sayHello(const char*);
	};	
	
	/*speak.cpp*/
	#include "speak.h"
	void Speak::sayHello(const char*)
	{
		std::cout<<"Hello "<<str<<endl;
	}
	
	/*helloworld.cpp*/
	#include "Speak.h"
	int main()
	{
		Speak speak;
		speak.sayHello("world");
		return 0;
	}
下面这条命令将上述两个源码文件编译链接成一个单一的可执行程序：

	g++ helloworld.cpp speak.cpp -o hellospeak

这里解释一下为什么在上述命令中没有提到“speak.h”文件，原因是：

**在“speak.cpp”中包含有“#include 'Speak.h'”这句代码，它的意思是搜索系统文件目录之前，将先在当前目录中搜索文件“Speak.h”。而该文件正在目录中，所以不用在命令中指定了。**	

####源文件生成对象文件
选项`-c`用来告诉编译器编译源代码但是不要执行链接，输出结果为对象文件。文件名默认与源代码文件名相同，只是将其后缀变为`.o`。

下面程序将编译源代码文件hellospeak.cpp并生成对象文件hellospeak.o:

	g++ -c hellospeak.cpp 
	
命令g++也能识别.o文件并将其作为输入文件传递给链接器。下列命令将编译源码文件为对象文件并将其链接成单一的可执行程序：

	g++ -c hellospeak.cpp -o hspk1.o
	g++ -c speak.cpp -o hspk2.o
	g++ hspk1.o hspk2.o -o hellospeak
####编译预处理

选项`-E`使g++将源代码用编译预处理器处理后不再执行其他动作。

	g++ -E helloworld.cpp
	
这条命令执行完后，头文件iostream被包含进来，而且它又包含了其他头文件，除此之外，还有若干个处理输入输出类的定义。

####生成汇编代码

	g++ -S helloworld.cpp
	
	
####当我们写程序的时候被隐藏了得过程

- 预处理
- 编译：生成汇编代码
- 汇编：生成机器指令
- 链接

######链接
链接的概念没有想象的辣么复杂，还是比较容易理解的。比如，我在程序模块`main.c`中使用另一个模块`func.c`中的函数`foo()`。

在`main.c`中的每一处调用`foo()`的地方都要知道`foo()`的函数地址，但是由于每个模块是单独编译的，所以，编译器在编译`main.c`的时候并不知道`foo()`函数的地址，所以它暂时把调用`foo()`的指令目标地址搁置，等待最后链接的时候由链接器去将这些指令的目标地址修正。

使用链接器，就可以直接饮用其他模块的函数和全局变量而无需知道他们的地址，因为链接器在链接的时候会根据你所饮用的符号foo，自动的去相应的fnuc.c模块查找foo的地址，然后将main.c中得所有引用到foo的指令修正，让他们的目标地址为真正的foo函数地址。

以上就是静态链接最基本的过程和作用。

######重定位

在链接过程中，对其他定义在目标文件中的函数调用的指令要被重新调整，对使用其他定义在其他目标文件中的变量来说，也存在同样的问题。

而修正变量地址的过程就叫做**重定位**，每个要被修正的地方叫一个重定位入口。
