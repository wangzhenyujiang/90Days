#Swift 数组

##数组构造语句

```Swift
var shoppinglist: [String] = ["Eggs","Milk"]
```
>注意：
>`shoppinglist` 数组被声明为变量（`var`关键字创建），而不是常量(`let`创建)是因为以后可能会有更多的数据项被插入其中。


由于Swift是类型推断机制，当我们用字面量构造只拥有相同类型值数组的时候，我们不必把数组的类型讲清楚：

```Swift
var shoppinglist = ["Eggs","Milk"]
```

##访问和修改数组

我们可以通过数组的方法和属性来访问和修改数组，或者下标语法。还可以使用数组的只读属性`count`来获取数组中的数据项数量。

```Swift
println("The shopping list contains \(shoppingList.count) items.")
```

使用`isEmpty`来检查 `count`属性的值是否为 0 ：

```Swift
if shoppinglist.isEmpty{
	println("The shoppinglist is empty")
}else{
	println("The shoppinglist is not empty")
}
```

也可以使用 `append` 方法在数组后面添加新的数据项：

```Swift
shoppinglist.append("Flour")
//shoppinglist 有三个数据项了
```

除此之外，使用加法赋值运算符 (+=) 也可以直接在数组后面添加一个或多个拥有相同类型的数据项：

```Swift
shoppinglist += ["Baking Powder"]

shoppinglist += ["Chocolate Spread","Cheese","Butter"]
```

可以使用下标语法来获取数组中的数据项，把我们需要的数据项的索引值放在直接放在数组名称的方括号中：

```Swift
var firstItem = shoppinglist[0]
```
调用数组的 `insert(atIndex:)` 方法在某个具体索引值之前添加数据项：

```Swift
shoppinglist.insert("Maple Syrup",atIndex: 0)
```
移除数组中的数据项：

```Swift
shoppinglist.removeAtIndex(0)

或者：

shoppinglist.removeLast()
```

##数组的遍历

```Swift
for item in shoppinglist{
	println(item)
}
```
如果我们需要把项的索引也打印出来，我们可以用内置的 `enumerate` 函数来进行数组遍历。

```Swift
for (index,value) in enumerate(shoppinglist){
	println("Item\(index + 1)：\(value)")
}
```

##创建并且构造一个数组

```Swift
var someItem = [Int]()

var ThreeDoubles = [Double](count: 3, repreateValue: 0.0)
var anotherThreeDoubles = [Double](count: 3,repeateValue: 2.5)

var sixDouble = ThreeDoubles + anotherThreeDouble
```