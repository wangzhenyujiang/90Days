#Creating Simple View Animations in Swift

##Basic View Animaitons

- center
- alpha
- frame
- bounds
- transfrom
- backgroundColor
- contentStretch

UIKit 提供了下面这些 API 方法来使用动画：

```Swift
UIView.animateWithDuration(_:,animations:)
UIView.animateWithDuration(_:,animations;,completion:)
UIView.animateWithDuration(_:,delay:,options:,animations:,completion:)
```

##Spring Animations

`Spring Animations` 动画会模拟显示生活中的一些类似弹簧的场景：

下面是方法：

```Swift
UIView.animateWithDuration(_:,delay:,usingSpringWithDamping:,initialSpringVelocity:,options:,animations:,completion:)
```

##Keyframe Animations

`Keyframe Aniamtions` 提供的 API 如下所示：

```Swift
UIView.animateKeyframesWithDuration(_:,delay:,options:,animations:)
UIView.addKeyframeWithRelativeStartTime(_:,relativeDuration:)
```
上面这两个方法是结合起来用的。

##View Transitions

这个方法是当你需要用一个新的 view 去替换另一个 view 的时候或者想从一个 view 上删除一个 view 的时候：

```Swift
UIView.transitionWithView(_:,duration:,options:,animations:,completion:)
UIView.transitionFromView(_:toView:,duration:options:,completion:)
```


