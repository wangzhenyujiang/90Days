#Swift闭包（Closures）

我认为Swift中闭包这个高大上的概念就是OC中`block`的替代品，熟悉OC的盆友就把它当做`block`来处理就好了。

###闭包表达式语法(Closure Expression Syntax)

```Swift
{ (parameters) -> returnType in
		statements
}
```
###参数名称缩写(Shorthand Argument Names)

Swift 自动为内敛函数提供了参数名称缩写功能，您可以直接通过`$0` `$1` `$2`来顺序调用闭包的参数。

如果您在闭包表达式中使用参数名称缩写，您可以在闭包参数列表中省略对其的定义，并且对应参数名称缩写的类型会通过函数类型进行推断。 `in`关键字也同样可以被省略，因为次时闭包表达式完全由必要函数体构成：

```Swift
reversed = sorted(names,$0 > $1)
```
###尾随闭包 (Trailing Closures)

如果您需要将一个很长的闭包表达式作为最后一个参数传递给函数，可以使用尾随闭包来增强函数的可读性。尾随闭包是一个书写在函数括号之后的闭包表达式，函数支持将其作为最后一个参数调用。

```Swift
func someFunctionThatTakesAClosure(closure: () -> ()){
	//函数主体部分
}

//不是尾随方式
someFunctionThatTakesAClosure({
	//闭包主体部分
})

//尾随方式
someFunctionThatTakesAClosure() {
	//闭包主体部分
}
```

当闭包非常长以至于不能在一行中进行书写时，尾随闭包变得非常有用。举例来说，Swift的 `Array` 类型有一个 `map` 方法，其获取一个闭包表达式作为其唯一参数。数组中的每一个元素调用一个该闭包函数，并返回该元素所映射的值(也可以是不同类型的值)。具体的映射方式和返回值类型由闭包来指定。

当提供给数组闭包函数后，`map` 方法将返回一个新的数组，数组中包含了与原数组一一对应的映射之后的值。

下面例子介绍了如何在`map`方法中使用尾随闭包将`Int`类型数组`[16,58,510]` 转换为包含`String`类型的数组`["OneSix","FiveEight","FiveOneZero"]`:

```Swift
let digitNames = [
	0:"Zero",1:"One",2:"Two",3:"Three",4:"Four"
	5:"Five",6:"Six",7:"Seven",8:"Eight",9:"Nine"
]
let numbers = [16,58,510]
```

您现在可以通过创建一个尾随闭包给`numbers`的`map`方法来创建对应的字符版本数组。需要注意调用时用`numbers.map`不需要在后面加任何括号，因为其只需要传递闭包表达式这一个参数，并且该闭包表达式参数通过尾随方式进行书写：

```Swift
let strings = numbers.map{
	(var number) -> String in
	var output = ""
	while number > 0 {
		output = digitNames[number % 10]! + output
		number /= 10
	}
	return output
}
```
`map` 在数组中为每一个元素调用了闭包表达式。 您不需要指定闭包的输入参数`number`的类型，因为可以通过要映射的数组进行推断。

闭包`number`参数被声明为一个变量参数,因此可以在闭包函数体内对其进行修改。 闭包表达式制定了返回类型为`String`,以表明存储映射值的新数组类型为`String`。

闭包表达式在每次被调用的时候创建了一个字符串并返回。其使用求余运算符(number % 10)计算最后一位数字并利用`digitNames`字典获取所有的映射的字符串。

>注意：字典`digitNames`下标后跟一个感叹号(!)，因为字典下标返回一个可选值(optional value),表明即使该值key不存在也不会查找失败。

上例中尾随闭包语法在函数后整洁封装了具体的闭包功能，而不再需要将整个闭包包裹在`map`函数的括号内。

