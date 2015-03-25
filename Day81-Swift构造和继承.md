#Swift 构造和继承

Swift 将为所有属性已提供默认值的且自身没有定义任何构造器的结构体或基类，提供一个默认的构造器。这个默认的构造器将简单的创建一个所有属性值都设置为默认值的实例。

####结构体的逐一成员构造器

除了默认构造器，如果结构体对所有存储属性提供了默认值且自身没有提供定制的构造器，它能自动获得一个**逐一成员构造器**。

>注意，如果你为某个值类型定义了一个定制的构造器，你将无法访问到默认构造器(如果是结构体，则无法访问到逐一对象构造器)。


##Example

```Swift
class Person{
	var name: String
	
	init(name: String){
		self.name = name
	}
	
	convience init(){
		self.init(name: "Unnamed")
	}
}

class Man : Person{
	var age: Int
	
	init(name: String,age: Int)
	{
		self.age = age
		super.init(name: name)
	}
	
	convience orride init(name: String){
		self.init(name: name, age: 20)
	}
}
```
`Man` 类的便利构造器因为和基类 `Person` 的指定构造器的参数是相同的，所以要写上 `override` 关键字。同时 `Man` 类也继承了 `Person` 类的便利构造函数，但是不同的是，当用 `init()` 初始化 `Man` 类的时候，将会调用 `Man` 的`init(name: String)`
方法。

这样，`Man` 类就会有三种方法来构造自己：

```Swift
var firstMan = Man()
var secondMan = Man(name: "Junny")
var thridMan = Man(name: "Junny",age: 21)

firstMan:  "Unnamed" 20
secondMan: "Junny" 20
thridMan: "Junny" 21
```