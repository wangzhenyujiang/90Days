#响应式编程Functional Reactive Programming


我的理解就是把监听变成了立即响应式的，一旦与自己相关联的元素发生了变化，那我自己会实时的发生改变。

如下的小程序：

	a = 2
	b = 2
	c = a + b //c is 4
	
	b = 3
	//now what is the value of c?
	
如果使用了函数式编程，`c`的值将会随着`b`的值改变而改变。

####在移动开发中的用处

在重交互的应用里是非常有用的。我觉得这也很好理解。

比如，在iOS开发中的`textFiled`查看当前正在输入的字数，用FRP就灰常合适。

在javascript中可以采用时间监听来处理，iOS中更多用的是delegate模式，本质上都是事件的分发和响应。

使用FRP主要有两个好处：直观和灵活。直观的代码容易编写、阅读和维护，灵活的特性便于是对变态的需求。

#ReactiveCocoa

ReactiveCocoa是github2013年开源的一个项目，是iOS平台上对FRP的实现。FRP的核心是信号，信号在RAC中是通过`RACSignal`来表示的，信号是数据流，可以被绑定和传递。

可以把信号想象成水龙头，只不过里面不是水，而是玻璃球(value)，直径跟水管的内径一样，这样就能保证玻璃球是依次排列，不会出现并排的情况(数据都是线性处理的，不会出现并发情况)。水龙头的开关默认是关的，除非有了接收方(subscriber)，才会打开。这样只要有新的玻璃球进来，就会自动传送给接收方。可以在水龙头上加一个过滤嘴(filter)，不符合的不让通过，也可以加一个改动装置，把球改变成符合自己的需求(map)。也可以把多个水龙头合并成一个新的水龙头(combineLatest:reduce:)，这样只要其中的一个水龙头有玻璃球出来，这个新合并的水龙头就会得到这个球。

下面看一个例子：

假如对象的某个属性想绑定某个消息，可以使用`RAC`这个宏，相当于给玻璃球套了一个水龙头。

	RAC(self.submitButton,enabled)=[RACSignal combineLatest:@[self.usernameField.rac_textSignal,self.passwordField.rac_textSignal]reduce:^id(NSString *userName,NSString *password){return @(userName.length >= 6 && password.length >= 6);}];
	
这样，如果用户名和密码框的长度都超过6，提交按钮就enable了，反之，如果没有符合要求，就会处于非开启状态。

可以看到`usernameField`有了一个新的属性`rac_textSignal`，这是RAC在`UITextField`category中添加的，直接使用即可。

`ReactiveCocoa`真的很神奇，有兴趣的同学可以去[项目主页](https://github.com/ReactiveCocoa/ReactiveCocoa)查看。

######Preferences:

[Limboy](http://limboy.me/ios/2013/06/19/frp-reactivecocoa.html)

[Tiger的小站](http://www.itiger.me/?p=38)