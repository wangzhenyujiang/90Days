title: "Swift Filter Reduce"
date: 2015-05-10 21:13:18
tags: Swift
---
Swift 函数式 API 很受开发者的喜欢，因为笔者大脑内存较小，在此记录一下几个函数式函数的使用：

##数组

首先定义一个数组，下面应该都会用到：

```Swift
var evens = [Int]()
for i in 1...10 {
	evens.appends(i)
}
LOG(evens)
```
###Functional Filtering

当你想把上面数组中的偶数全部取出来的时候会怎么写呢？一般会这样写：

```Swift
for i in evens {
	if i % 2 == 0 {
		LOG(i)
	}
}
```

那通过我们的的 `Filter` 会咋么写嘞？

Duang...

```Swift
evens.filter(num in num % 2 == 0)
```

#####个人理解的 Filter 

Swift 数组的 filter 函数接收一个闭包，这个闭包是这个样子的：

```Swift
{(T) -> Bool}
```
也就是说，filter 函数会在数组的每一个元素上运行一遍，并把闭包返回为 `true` 的那个元素留在数组内把返回为 `false` 的那个元素丢掉。

#####自己实现一个数组的 Filter

```Swift
func myFilter(source: [Int], predicate: (Int) -> Bool){
	for item in source {
		if predicate(item) {
			result.append(item)
		}
	}
	return result
}
```
###Reducing

现在，你已经过滤出所有的偶数了，那我现在想把所有的偶数加起来怎么办？你会这样写的吧：

```Swift
var evens = [Int]()
for i in 1...10 {
	if i % 2 == 0 {
		evens.append(i)
	}
}
var evenSum = 0
for i in evens {
	evenSum += i
}
LOG(evenSum)
```

接下来我们 duang 一下函数式该怎么写，当然我们会用到 `Swift` 的函数式 **API** 的 `reduce` 函数：

```Swift
evenSum = Array(1...10).filter { num in num % 2 == 0}.reduce(0){(total, num) in total + num}
LOG(evenSum)
```
#####我理解的 `reduce` 函数
`reduce` 函数首先接收两个参数并返回一个和第一个参数同类型的值，第一个参数是一个值类型，第二个参数是一个闭包，返回也是一个值类型并和第一个参数的类型相等：

```Swift
func reduce(initinal: U, combine: (U, T) -> U) -> U)
```
首先你要传入第一个参数，一个初始的要返回的值，然后第二个参数是个闭包，闭包是这样的：

```Swift
{(U, T) -> U}
```
数组的每个元素将会在这个闭包上运行一遍，并返回处理后的闭包的第一个元素。最后，整个 `Reduce` 函数返回自己的第一个参数。

#####自己实现 Reduce 函数

```Swift
extension Array {
	func myReduce<T, U>(seed: U, combine: (U, T) -> U) -> U){
		var current = seed
		for item in self {
			current = combine(current, item)
		}
		return current
	}
}
```

###Map

map 函数接受一个闭包，这个闭包是这样的：

```Swift
map<T, U>(tranform: (T) -> U)
```

也就是说，map 函数是把一个元素对应成另一种元素。
##最后
总体来说，这些都是 `Swift` 闭包的有趣运用,真的太 mazing 了，有一个特别好丸的图来理解它们，哈哈：

![](http://ww2.sinaimg.cn/bmiddle/62d02ba8jw1ery92n4uylj20hl0d5jt5.jpg)

