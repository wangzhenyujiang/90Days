#autorelease与autoreleasepool

虽然说这两个技术已经成为了过去式(ARC所替代)，但是看看前人的智慧还是很有必要的。

####autorelease基本用法

- 对象执行`autorelease`方法时会将对象添加到自动释放池中
- 当自动释放池销毁时自动释放池中的所有对象作`release`操作
- 对象执行`autorelease`方法后自身引用计数器不会改变，而是返回对象本身。

####autorelease优点

- `autorelease`实际上只是对`release`的调用延迟了，也就是说当该pool释放的时候，该pool中的所有对象会被调用`release`

####autorelease使用的时候应该注意
- 操作对象占用的内存比较大的时候不要用`autorelease`，因为担心对象释放的时间太迟导致内存不足
- 对占用内存比较小的对象可以使用

####autoreleasepool自动释放池
自动释放池遵循栈的“先进后出”原则：

	import <Foundation/Foundation.h>
	#import "Person.h"
	
	int main(int argc,const char * argv[])
	{
		@autoreleasepool
		{
			Person *person=[[[Person alloc]init]autorelease];
			
			@autoreleasepool
			{
				Person *person2=[[[Person alloc]init]autorelease];
			}
			
			Person *person=[[[Person alloc]init]autorelease];
		}
		return 0;
	}
	
所以，释放的时候应该是里面的`Person2`先释放。

##那么，问题就来了，Autorelease对象什么时候释放？

在这里借用[sunnyxx](http://blog.sunnyxx.com/2014/10/15/behind-autorelease/)的一句话：

>在没有手动加`Autorelease Pool`的情况下，`Autorelease`对象是在当前的`runloop`迭代结束的时候释放的，而它能够释放的原因是系统在每个`runloop`中都加入例如自动释放池Push和Pop.

##Autorelease原理


####AutoreleasePoolPage

ARC下，我们使用`@autoreleasepool{}`来使用一个AutoreleasePool，随后编译器将其改写成下面的样子：

	void *context=objc_autoreleasePoolPush();
	//{}中的代码
	objc_autoreleasePoolPop(context);
	
而这两个函数都是对`AutoreleasePoolPage`的简单封装，所以自动释放机制的核心就在于这个类。

`AutoreleasePoolPage`是一个C++实现的类：

![](http://ww2.sinaimg.cn/mw690/51530583gw1elj2ugt21wj20f109m3zl.jpg)

- AutoreleasePool并没有单独的结构，是由若干个AutoreleasePoolPage以双向链表的形式组合而成的
- AutoreleasePool是按着线程一一对应的(结构中的thread指针指向当前的线程)
- `id *next`指针指作为游标指向栈顶最新add进来的autorelease对象的下一个位置
- 一个AutoreleasePoolPage的空间被沾满的时，会新建一个AutoreleasePoolPage对象，连接链表

**所以，向一个对象发送`-autorelease`消息，就是将这个对象加入到当前的AutoreleasePoolPage栈顶的next指针指向的位置。**

####释放时刻

每当进行一次`objc_autoreleasePoolPush`调用时，runtime向当前的AutoreleasePoolPage中add进一个`哨兵对象`，值为0（也就是个nil），那么一个page一下子就变成了这样：
![](http://ww2.sinaimg.cn/large/51530583gw1elj5z7hawej20ji0dewff.jpg)

`objc_autoreleasePoolPush`返回的是这个哨兵对象的地址，被`objc_auroreleasePoolPop()`作为入参，于是：

- 根据传入的哨兵对象地址找到哨兵对象所处的page
- 在当前的page中，将晚于绍币ing对象插入的所有autorelease对象都发送一次`-release`消息，并向回移动`next`指针到正确位置

刚才的objc_auroreleasePoolPop执行后，最终变成了下面的样子：

![](http://ww3.sinaimg.cn/mw690/51530583gw1elj6u2i3fyj20dz0bqdgi.jpg)

##嵌套的autoreleasePool

知道了上面的原理，嵌套的AutoreleasePool就非常简单了，婆婆的时候总会释放到上次push的位置为止，多层的pool就是多个哨兵对象而已。