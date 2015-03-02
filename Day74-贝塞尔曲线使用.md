#UIBezierpath 使用简介

###创建矩形

```Swift
init(rect rect: CGRect) -> UIBezierpath
```

	(UIBezierPath *)bezierPathWithRect:(CGRect) rect

###创建原型或者椭圆

```Swift
init(ovalInRect rect: CGRect) -> UIBezierPath
```
	(UIBezierPath *)bezierPathWithOvalInRect:(CGRect) rect

这个方法根据传入的 rect 矩形参数绘制一个内切曲线。
当 rect 传入的是一个正方形的时候，绘制的是一个圆；当传入的 rect 是一个长方形的时候，绘制出的是一个椭圆。

###使用贝塞尔曲线创建一个弧形

```Swift
init(  arcCenter center: CGPoint,
         redius  radius: CGFloat,
 startAngle  startAngle: CGFloat,
      endAngle endAngle: CGFloat,
    clockwise clockwise: BOOL ) -> UIBezierPath
```

	(UIBezierPath *)bezierPathWithArcCenter:(CGPoint)center
                                   radius:(CGFloat)radius
                               startAngle:(CGFloat)startAngle
                                 endAngle:(CGFloat)endAngle
                                clockwise:(BOOL)clockwise
                                

其中的参数分别指定：这段圆弧的中心，半径，开始角度，结束角度，是否是顺时针方向。

下图为弧线参考系: ![](http://img.blog.csdn.net/20130904200813921?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY3JheW9uZGVuZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

###绘制二次贝塞尔曲线

```Swift
func addQuadCurveToPoint(_ endPoint: CGPoint,
          controlPoint controlPoint: CGPoint)
```

	-(void)addQuadCurveToPoint:(CGPoint)endPoint
	              controlPoint:(CGPoint)controlPoint
	            

![点的关系](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIBezierPath_class/Art/quadratic_bezier_curve.jpg)

###绘制三次贝塞尔曲线

```Swift
func addCureToPoint(_ endPoint: CGPoint,
   controlPoint1 controlPoint1: CGPoint,
   controlPoint2 controlPoint2: CGPoint)
```

	-(void)addCurveToPoint:(CGPoint)endPoint
	         controlPoint1:(CGPoint)controlPoint1
	         controlPoint2:(CGPoint)controlPoint2

![](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIBezierPath_class/Art/uibezier_curve.jpg)