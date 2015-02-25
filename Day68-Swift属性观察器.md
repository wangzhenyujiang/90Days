#Swift属性观察器

属性观察器监控和相应属性值的变化，每次属性被设置值的时候都会调用属性观察器，甚至新的值和现在的值相同的时候也不例外。

可以为除了延迟存储属性之外的其他存储属性添加属性观察器，也可以通过重载属性的方式为继承的属性(包括存储属性和计算属性)添加属性观察器。


>不需要为无法重载的计算属性添加属性观察器，因为可以通过setter 直接监控和相应值的变化。

可以为属性添加如下的一个或全部观察器：

- `willSet`在设置新的值之前调用
- `didSet`在新的值被设置后立即调用

`willSet`观察器会将新的属性值作为固定参数掺入，在`willSet`的实现代码中可以为这个参数指定一个名称，如果不指定则参数仍然可用，这时使用默认名称`newValue`表示。

类似的，`didSet`观察器会将旧的属性值作为参数传入，可以为该参数命名或者使用默认参数名`oldValue`。

>`willSet` `didSet`观察器在属性初始化过程中不会被调用，它们只会当属性的值在初始化之外的地方被设置时调用。

下面是一个`willSet`和`didSet`的实际例子，其中定义了一个名为`StepCounter`的类，用来统计当人步行时的总步数：

```Swift
class StepCounter {
	var totalSteps: Int = 0 {
		willSet(newTotalSteps){
			println("About to set totalSteps to \(newTotalSteps)")
		}
		didSet {
			if totalStep > oldValue {
				println("Added \(totalSteps - oldValue) steps")
			}
		}
	}
}

let stepCounter = StepCounter()
setpCounter.totalSteps = 200

setpCounter.totalSteps = 360

stepCounter.totalSteps = 896
```

`setpCounter`类定义了一个`Int`类型的属性，包含`willSet`和`didSet`观察器。

当`totalSteps`设置新值的时候，它的`willSet`和`didSet`都会被调用，甚至当新的值和现在的值完全相同也会调用。

>如果在`didSet`观察器里为属性赋值，这个值会替换观察器之前设置的值。