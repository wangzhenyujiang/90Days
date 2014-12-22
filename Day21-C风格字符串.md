#C风格字符串

####C风格字符串的使用
C++语言通过(const)char *类型的指针来操纵C风格字符串。一般来说，我们使用指针的算术操作来遍历C风格字符串，每次对指针进行测试并进行递增1，直到到达结束符`NULL`为止。

	const char *cp="some value";
	while(*cp)
	{
		cout<<*cp;
		++cp;
	}
	
**注意：如果cp所指向的字符串数组没有`NULL`结束符，则次循环失败。这时，cp会从指向的位置开始读数，直到遇到内存中某处`NULL`为止。**

####C风格字符串的标准库函数

- `strlen(s)`返回s的长度，不包括字符串结束符NULL

- `strcmp(s1,s2)`比较两个字符串s1和s2是否相同，如果相同则返回0，如果s1大于s2则返回正数，反之则返回负数。

- `strcat(s1,s2)`将s2拼接到s1上，并返回s1。

- `strcpy(s1,s2)`将s2复制给s1,并返回s1。

- `strncat(s1,s2,n)`将s2的前n个字符连接到s1后面，并返回s1。

- `strncpy(s1,s2,n)`将s2的前n个字符复制给s1，并返回s1。

####永远不要忘记字符串结束符NULL

在使用处理C风格字符串的标准函数时，牢记字符串必须以结束符NULL结束。

14.12.22
####我他妈真是个白痴
唉~真是说和做是两回事啊，写代码又出错了，还是白痴似的错误，简直了！

下面是我的MineString字符串类的初始化函数其中之一：

	MineString::MineString(const char *str)
	{
		_size=strlen(str);
		_string=new char [_size+1];
		strcpy(_string,str);
		_string[_size]=NULL;
	}
	
解释一下：

这是针对MineString("Hello")这种类型的初始化而设计的初始化函数。首先，我们获得了传入字符串的size，为了存储这段字符串，我们向内存申请了内存空间，但是是size+1个char的空间，因为**最后一位我们要存储null来标注字符串结束**。

下面是我的输出函数：

	void MineString::MinePrint()
	{
		if(_size!=0)
		{
			while(_string)
			{
				cout<<*(_string);
				_sring++;
			}
			_string=_string-_size;
		}
	}
	
小伙伴们发现**错误**没有？！我知道你们都发现了=。=

2B铅笔的我竟然拿(_string)当做循环条件，大哥啊，那个是地址啊...心酸的我眼泪掉下来...所以正确的应该是：

	void MineString::MinePrint()
	{
		if(_size1=0)
		{
			while(*(_string))
			{
				cout<<*(_string);
				_string++;
			}
			_string=_string-_size;
		}
		
	}
我们应该用最后一个字符是null来判断字符串是否结束。
