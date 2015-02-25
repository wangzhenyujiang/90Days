#Swift函数

###多重返回值函数(Functions with Mutiple Return Values)

你可以用元组(Tuple)类型让多个值作为一个复合值从函数中返回。

下面的这个例子中，`count`函数用来计算一个字符串中元音，辅音和其他字母的个数。

```Swift
func count(string: String) -> (vowels: Int,consonants: Int,other: Int){
	var vowels = 0,consonants = 0,others = 0
	for character in string {
		//code
	}
	return (vowels,consonants,others)
}
```
你可以用`count`函数来处理任何一个字符串，返回的值将是一个包含三个`Int`型值的元组(tuple):

```Swift
let total = count("some string");
println("\(total.vowels)")
```

需要注意的是，元组的成员不需要在函数中返回时命名，因为它们的名字已经在函数返回类型中有了定义。

###外部参数名(External Parameter Names)

有时候，调用函数时，给每个参数命名是非常有用的，因为这些参数名可以指出各个实参的用途是什么。

如果你希望函数的使用者在调用函数时提供参数名字，那就需要给每个参数除了局部参数名外再定义一个`外部参数名`。外部参数名写在局部参数名之前，用空格分隔。

```Swift
func someFunction(externalParameterName localParamterName: Int){
	//code
}
```
>注意：如果你提供了外部参数名，那么函数在被调用时，必须使用外部参数名。

一下是个例子，这个函数使用一个结合者函数把两个字符串联在一起：

```Swift
func join(s1: String, s2: String, joiner:String) -> String{
	return s1 + joiner + s2
}
```

当你调用这个函数时，这三个字符串的用途是不清楚的：

```Swift
join("hello","world",",")
```
为了让这些字符串的用途更为明显，我们为`join`函数添加外部参数名：

```Swift
func join(string s1: String, toString s2: String, withJoiner joiner: String) -> String {
	return s1 + joiner + s2
}
```
现在，你可以使用这些外部参数名以一种清晰地方式来调用函数了：

```Swift
join(string: "hello", toString:"world",withJoiner:",")
```
这样，代码的可读性就大大增强。

###简写外部参数名(Shorthand External Parameter Names)

如果你想要提供外部参数名，但是局部参数名已经定义好了，纳尼不需要写两次参数名。相反，只写一次参数名，并用`#`号作为前缀。这告诉Swift使用这个参数名作为局部和外部参数名。

下面这个例子定义一个叫 `containsCharacter`的函数，使用 `#`的方式定义了外部参数名：

```Swift
func containsCharacter(#string: String, #characterToFind: Character) -> Bool {
	for character in string {
		if character == characterToFind {
			return true
		}
	}
	return false
}
```

##函数类型(Function Types)

每个函数都有中特定的函数类型，由函数的参数类型和返回类型组成。

例如：

```Swift
func addTwoInts(a: Int, b: Int) -> Int {
	return  a + b
}

func multiplyTwoInts(a: Int, b: Int) -> Int {
	return a * b
}
```

这两个函数的类型是 `(Int, Int) -> Int`，可以读作“这个函数类型，它又两个`Int`型的参数并返回一个`Int`型的值。”

下面是一个例子，一个没有参数，也没有返回值的函数：

```Swift
func printHelloWorld() {
	println("hello,world")
}
```
这个函数的类型是`() -> ()`，或者叫做“没有参数，并返回`Void`类型的函数”。

###使用函数类型(Using Function Types)

在Swift中，使用函数类型就像使用其他类型一样。例如，你可以定义一个类型为函数的常量或者变量，并将函数赋值给它：

	var mathFunction: (Int,Int) -> Int = addTwoInts

现在你可以用`mathfunction`来调用被赋值的函数了：

	println("Reault:\(mathFunction(2,3))")

有相同匹配类型的不同函数可以被赋值给同一个变量，就像非函数类型的变量一样：

	mathFunction = multiplyTwoInts
	println("Result: \(mathFunction(2,3))")

就像其他类型一样，当赋值一个函数给常量或变量时，你可以让Swift来推断函数类型：

	let anotherMathFunction = addTwoInts

###函数类型作为参数类型(Function Types as Paramter Types)

你可以用`(Int, Int) -> Int`这样的函数类型作为另一个函数的参数类型。在这样你可以将函数的一部实现交给函数的调用者。

下面是一个例子：

	func printMathResult(mathFunction: (Int, Int) -> Int,a: Int, b: Int){
		println("Result:\(mathFuntion(a, b))")
	}
	
	printMathResult(addTwoInts, 3, 5)

`printMathResult` 函数的作用就是输出另一个合适类型的数学函数的调用结果。它不关心传入函数是如何实现的，它只关心这个传入的函数类型是正确的。这使得`printMathResult` 可以以一种类型安全(type-safe)的方式来保证传入函数的调用是正确的。

###函数类型最为返回类型(Function Types as Return Types)

你可以用函数类型那个作为另一个函数的返回类型。你需要做的是在返回箭头 (`->`)后写一个完整的函数类型。