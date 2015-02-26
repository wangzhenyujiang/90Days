#Swift中的!和?

一般我们在以下两种情况下会遇到!和?的使用

1.声明变量时：

```Swift
var number : Int?
var str : String!
```

2.对变量操作时：

```Swift
number?.hashValue
str!.hashValue
```

#####1.声明变量时

在声明一个变量时如果不手动初始化，Swift不会自动初始化该变量为一个默认的值。

```Swift
var a : String
var b = a        //error:因为没有初始化a，a没有值
```

但是对于Optional的变量则不同，Optional的变量在声明时如果不初始化，Swift会自动将该变量初始化为nil。声明变量时在类型后添加?或者!就是告诉编译器这个一个Optional的变量，如果没有初始化，你就讲其初始化为nil

```Swift
var a : String?    //a 为nil
var b : String!    //b 为nil
var a_test = a     //a_test为nil
var b_test = b     //b_test为nil
```

Optional事实上是一个枚举类型，从下图可以看出，Optional包含None和Some两种类型，而nil就是Optional.None，非nil就是Optional.some。如果Optional变量在声明时不初始化，Swift会调用init()来初始化变量为nil，而用非nil的值初始化变量时，会通过Some(T)把该原始值包装，所以在之后使用的时候我们需要通过解包取出原始值才能使用。

![](http://segmentfault.com/img/bVco3y/view)

#####2. 对变量进行操作时

	var arrayCount = dataList?.count

这时问号的意思类似于isResponseToSelector,即如果变量是nil，则不能响应后面的方法，所以会直接返回nil。如果变量非nil，就会拆Some(T)的包，去除原始值执行后面的操作。

	var arrayCount = dataList!.count

这里的叹号和之前的问号则不同，这里表示我确定dataList一定是非nil的，所以直接拆包取出原始值进行处理。因此此处如果不小心让dataList为nil，程序就会crash掉。

####回到上面声明时？和！区别的问题上去
声明变量时的？只是单纯的告诉Swift这是Optional的，如果没有初始化就默认为nil，而通过！声明，则之后对该变量操作的时候都会隐式的在操作前添加一个！。

###总结

1. 问号？
a.声明时添加？，告诉编译器这个是Optional的，如果声明时没有手动初始化，就自动初始化为nil
b.在对变量值操作前添加？，判断如果变量时nil，则不响应后面的方法。
2. 叹号！
a.声明时添加！，告诉编译器这个是Optional的，并且之后对该变量操作的时候，都隐式的在操作前添加！
b.在对变量操作前添加！，表示默认为非nil，直接解包进行处理