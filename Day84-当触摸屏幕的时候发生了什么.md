#Day84-当触摸屏幕的时候发生了什么

当屏幕接收到一个 touch 的时候， iOS需要找到一个合适的对象来处理事件 (touch 或者手势)， 要寻找这个对象，需要用到这个方法：

```
func hitTest(point: CGPoint, event: UIEvent) -> UIView
```
该方法会首先在 `application` 的 `keywindow` 上调用 (`UIWindow` 也是 `UIView` 的子类)，并且该方法的返回值将用来处理事件。如果这个 `view` (无论是 `window` 还是普通的 UIView) 的 `userInterationEnabled` 属性被设置为 `false`， 则它的 `hitTest:` 永远返回 `nil`, 这就意味着它和他的子视图没有机会去接受和处理事件。如果 `userInterationEnabled` 被设置为 `true`，则会先判断产生触摸的 point 时候发生在自己的 `bounds` 内，如果没有也将返回 nil; 如果 point 在自己的范围内，则会为自己的每个子类视图调用 `hitTest:` 方法，只要有一个子视图通过这个方法返回一个 `UIView` 对象; 如果没有子视图返回 `UIView` 对象，则父视图将会把自己返回。

所以，在事件分发中，有这个几个关键点：

- 如果父类不能相应事件(userInteractionEnabled 为 false),则其子类视图也将无法相应事件。
- 如果子视图的 frame 有一半在外面，则在外面的部分是无法相应事件的，因为它超出了视图的范围。
- 整个时间链只会返回一个 `Hit-Test View` 来处理事件。
- 子视图的顺序会影响到 `Hit-Test View` 的选择：最先通过 `hitTest:` 方法返回的 `UIView` 才会被返回，假如有两个子视图平级，并且它们的 `frame` 一样，但是谁是后添加的谁就优先返回。

了解了事件分发的这些特点后，还需要知道最后一件事，`UIView` 如何判断产生事件的 `point` 是否在自己的范围内？ 答案是通过 `pointInside` 方法，这个方法的默认实现方式类似于这样：

```
- (BOOL) pointInside: (CGPoint) point withEvent:(UIEvent *)event {
	return CGRectContainsPoint(self.bounds, point);
}
```

所以，当我们想该百年一个 view 的事件触发范围的时候，重写 `pointInside` 方法就可以了。