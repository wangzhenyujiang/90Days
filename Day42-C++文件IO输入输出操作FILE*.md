#C++文件IO输入输出操作File* 

最近研究C++的文件操作，发现了两种方式，一种是`File`一种是`fstream`，现在就小记一下：

##流式文件操作
这种方式的文件操作有一个重要的结构`FILE`,`FILE`在头文件`stdio.h`中定义如下：

	typedef struct
	{
		int level;
		unsigned flags;
		char fd;
		unsigned char hold;
		int bsize;
		unsigned char_FAR *buffer;
		unsigned char_FAR *curp;
		unsigned istemp;
		short token;
	}FILE;

`FILE`这个结构包含了文件操作的基本属性，**对文件的操作通过这个结构的指针来进行**，这种文件操作常用的函数见下：

- `fopen()`打开流
- `fclose()`关闭流
- `fputc()`写一个字符到流
- `fgetc()`从流中读一个字符

- `fseek()`在流中定位到指定的字符
- `fputs()`写字符串到流
- `fgets()`从流中读取一行或指定个字符

- `feof()`到达文件尾时返回true
- `ferror()`发生错误时返回真值

- `fread()`从流中读取指定个数的字符
- `fwrite()`向流中写指定个数的字符

####函数详解

1）`fopen()`

`fopen()`的原型是：

	FILE* fopen(const char *filename,const char* mode);
	
为使用而打开一个流，把一个文件和此流相连接，给此流返回一个`FILE`指针。参数filename指向要打开的文件名，mode表示打开状态的字符串，mode的值表如下：

- r 打开只读文件，该文件必须存在。 
- r+ 打开可读写的文件，该文件必须存在。 
- rb+ 读写打开一个二进制文件，只允许读写数据。
- rt+ 读写打开一个文本文件，允许读和写。
- w 打开只写文件，若文件存在则文件长度清为0，即该文件内容会消失。若文件不存在则建立该文件。 
- w+ 打开可读写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。 
- a 以附加的方式打开只写文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾，即文件原先的内容会被保留。（EOF符保留） 
- a+ 以附加方式打开可读写的文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾后，即文件原先的内容会被保留。 （原来的EOF符不保留）
- wb 只写打开或新建一个二进制文件；只允许写数据。
- wb+ 读写打开或建立一个二进制文件，允许读和写。
- wt+ 读写打开或着建立一个文本文件；允许读写。
- at+ 读写打开一个文本文件，允许读或在文本末追加数据。
- ab+ 读写打开一个二进制文件，允许读或在文件末追加数据。

2）`fclose()`

`fclose()`的功能就是关闭用`fopen()`打开的文件，其原型是：

	int fclose(FILE* fp);
	
如果成功，返回0，失败返回EOF。

3）`fputc()`
向流写一个字符，原型是：

	int fputc(int C,FILE *stream);
	
成功就返回这个字符，失败返回EOF。

	fputc("C",fp);
	
4)`fgetc()`
从流中读取一个字符，原型是:
	
	int fgetc(FILE *stream);
成功则返回这个字符，失败返回EOF。

	char ch1=fgetc(fp);
5)`fputs()`
写一个字符串到流中，原型是：

	int fputs(const char *s,FILE *stream);
	
例如：

	fputs("I love you",fp);
6)`fgets()`
从流中读取一行或指定个字符，原型是:

	char *fgets(char *s,int n,FILE *stream);
	
从流中读取n-1个字符，除非读完一行，参数s是来接收字符串，如果成功则返回s的指针，否则返回NULL。

例如：一个文档中有内容：

	Love,I Have
用fget(str1,4,file1);则执行后str1="Lov"，读取了4-1=3个字符，如果用fgets(str1,23,file1),则读取了一行(不包括行尾的'\n')。



