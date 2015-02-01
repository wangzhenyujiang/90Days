#Custom Drawing

你的用户期望一个漂亮的，有趣的和简单的用户界面。这取决于你的实现。无论你的应用有多么强大的功能，如果你的界面看上去很过时，你将很难取得好的用户量。这节有将介绍很多漂亮的颜色和Flash动画。一个真正漂亮的炫酷的界面是一个人性化app的重要的一部分。把精力放在用户体验上是可以写出一个完美应用的关键。

在这一节中，你将会学习几个iOS的drawing system，比如`UIKit`和`Core Graphics`。在这一节的最后，你将会掌握`UIKit Drawing cycle`,`drawing coordinate system`,`graphic contexts`,`paths`,`transforms`。你会知道如何让通过正确的师徒配置，高速缓存，刑诉排列，和使用层的优化您的绘图速度。你将能够避免可以避免的预渲染的图形使你的程序包边的膨胀。

使用正确的工具，你可以让你的APP有着高炫酷，低内存和很小的安装包。


##iOS的Drawing Systems

- UIKit：这是最高级别的，只在Objective-C中。它提供了很简单的实现，包括layout，compositing,drawing,fonts,images,animation还有更多。你很容易辨认出它的元素通过前缀UI，比如UIView和UIBezierPath.UIKit还继承`NSString`来提供一些简单的文本的绘制，就像`drawInRect:withAttributes:`等方法。
- Core Graphics (also called Quartz 2D)：在UIKit之下的一个基本的drawing system,`Core Graphics`将会是你自定义View的时候用的多的。`Core Graphics`已经很好的与`UIView`以及部分UIKit结合在了一起。`Core Graphics`数据结构和方法可以很好的被辨认出来通过CG前缀。
- Core Animation：这个系统提供了强大的二维和三维的动画服务。它也和UIView高度整合。
- Core Image：这个技术在iOS5出现的，`Core Image`提供了非常快的图像过滤，比如剪裁，锐化，扭曲和你能想象到的任何旋转。
- OpenGL ES：写应用不怎么用，主要用字游戏渲染上。