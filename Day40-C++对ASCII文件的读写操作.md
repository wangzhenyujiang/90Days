#C++对ASCII文件的读写操作
如果文件的每一个字节中均以`ASCII`代码形式存放数据，即一个字节存放一个字符，这个文件就是`ASCII`文件(或称字符文件)。程序可以从`ASCII`文件中读取若干个字符，也可以向它输出一些字符。

对`ASCII`文件的读写操作可以用以下两种方法：

- 用流插入运算符`<<`和流提取运算符`>>`输入输出标准类型的数据。`<<`和`>>`都已在iostream中被重载用于ostream和istream类对象的标准类型的输入输出。由于ifstream和ofstream分别是ostream和istream类的派生类，因此他们从ostream和istream类继承了公用重载函数，所以在对磁盘文件的操作中，可以通过文件流对象和流插入运算符`<<`及流提取运算符`>>`实现对磁盘文件的读写，如同cin、cout、和<<、>>对标准设备进行读写一样。
- 用文件流的put、get、getline等成员函数进行字符的输入输出。**C++用get()函数读入一个字符和getline()函数读入一行字符**。

看下面一个例子：

	#include<fstream>
	using namespace std;
	int main()
	{
		int a[10];
		ofstream outfile("f1.dat");
		if(!outfile)
		{
			cout<<"open error"<<endl;
			exit(1);
		}
		cout<<"enter 10 integer numbers:"<<endl;
		for(int i=0;i<10;i++)
		{
			cin>>a[i];
			outfile<<a[i]<<" ";
		}
		openfile.close();
		return 0;
	}
	
在程序中用`cin>>`从键盘逐个读入10个整数，每读入一个就将该数向磁盘文件输出，输出的语句为：outfile<<a[i]<<" ";
可以看出，用法和向显示器输出是相似的，只是把标准输出流对象cout换成文件输出流对象outfile而已。由于是向磁盘文件输出，所以在屏幕上看不到输出结果。

看下面一个程序，读取文件中的整数放在数组中，找出并输出个数中最大者和它在数组中的序号：

	#include<fstream>
	using namespace std;
	int main()
	{
		int a[10],max,i,order;
		ifstream infile("f.dat");
		
		if(!infile)
		{
			cout<<"open error"<<endl;
			return 1;
		}
		for(i=0;i<10;i++)
		{
			infile>>a[i];//从磁盘中读入10个整数
			cout<<a[i]<<" ";
		}
		cout<<endl;
		max=a[0];
		order=0;
		for(i=1;i<10;i++)
		{
			if(a[i]>max)
			{
				max=a[i];
				order=i;
			}
		}
		cout<<"max ="<<max<<" order = "<<order<<endl;
		infile.close();
		return 0;
	}
	