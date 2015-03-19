#AutoReleasePool

百度电面被问到这个问题了，当时胡扯了一大通然后扯到 ARC 上去了，结果面试官顺势问起了 ACR 是怎么实现了，心中一万一 XXX 奔过啊，算了，都是泪！

##ARC 与 MRC

MRC 和 ARC ，分别对应着手动引用计数和自动引用计数。

>注意，计数，并不是垃圾回收机制。也就是说，在 Objective-C 的开发中， ARC 并不像 Java 那样有 GC 做的垃圾回收机制，所以本质上还需要“手动”管理内存的。也就是说，我们在 ARC 下写的代码，不用自己手动插入 “retain”、“release” 这些消息，ARC 会在编译的时候为我们在合适的位置插入，释放不必要的内存。

而 `@autoRelesesPool` 就跟 对象的 `release` 密切相关。

##@autoreleasepool 干了啥

在 MRC 时代，如果我们，如果我们想先 retain 一个对象，但是并不知道在什么时候可以 release 它，我们可以像下面这么做：

```Objective-C
NSAutoreleasePool *pool = [[NSAutoreleasePool alloc]init];

NSString * str =[[NSString alloc]initWithString@"mazingyu"] autorelease];
//use str

[pool release];
//str is released
```
就是说，我们可以在创建对象的时候个对象发送 "autorelease" 消息，然后当 NSAutoreleasePool 结束的时候，“标记”过 autorelease 的对象都会被 “release” 掉，也就是会被释放掉。

但是在 ARC 时代，我们不用手动发送 autorelease 消息，ARC 会自动帮助我们加。而这个时候，`@autoreleasepool`做的事情和 NSAutoreleasePool 就一模一样了。

##什么时候用 @autoreleasePool

- 写基于命令行的程序时，就是没有 UI 框架，如 Appkit 等 Cocoa  框架时。
- 写循环，循环里面包含了大量临时创建的对象。
- 创建了新的线程
- 长时间在后台运行的任务

