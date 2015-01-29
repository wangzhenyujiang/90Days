#后台运行

当应用被按Home键退出后，应用仅有最多5秒钟的时间做一些保存或清理资源的工作。但是应用可以调用UIApplication的`beginBackgroundTaskWithExpirationHandler`方法，让应用最多10分钟的时间在后台长久运行。这个时间可以用来做清理本地缓存、发送统计数据等工作。

让程序在后台长久运行的代码如下：

	//AppDelegate.h文件
	@property (weak,nonatomic)UIBackgroundTaskIdentifier backgroundUpdateTask;
	
	//AppDelegate.m文件
	-(void)applicationDidEnterBackGround:(UIApplication *)application
	{
		[self beginBackgroundUpdateTask];
		
		//你的代码
		
		[self stopBackgroundUpdateTask];
	}
	
	-(void)beginBackgroundUpdateTask
	{
		self.backgroundUpdateTask=[
		[UIApplication shareApplication]
		beginBackgroundTaskWithExpirationHandler:^{
			[self endBackgroundUpdateTask];
		}];
	}
	
	-(void)endBackgroundUpdateTask
	{
		[[UIApplication shareApplication]endBackgroundTask:self.backgroundUpdateTask];
		self.backgroundUpdateTask=UIBackgroundTaskInvalid;
	}