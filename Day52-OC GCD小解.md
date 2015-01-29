#GCD小解

以下内容取自[@唐巧boy](http://blog.devtang.com/)的《iOS进阶》，仅作为个人的学习笔记。

##GCD之前

如果我们需要在网上下载一个东西，为了不阻塞主线程，那我们应该会这样：

	static NSOperationQueue *queue;
	
	- (IBAction)buttonClick:(id)sender
	{
		self.indicator.hidden = NO;
		[self.indicator startAnimating];
		queue = [[NSOptionQueue alloc] init];
		
		NSInvovationOperationQueue * op=
		[[[NSInvocationOperation alloc]initWithTarget:self selector:@selecto	r(download) object:nil]autorelease];
		
		[queue addOperation:op];
	}
	
	- (void)download{
		NSURL *url =[NSURL URLWithString:@"http://www.hao123.com"];
		NSError *error;
		NSString *data=[NSString stringWithContentsOfURL:url encoding:NSUTF8StringEncoding error:&error];
		
		if(data!=nil)
		{
			[self performSelectorOnMainThread:@selector(
			download_completed:) withObject:data
			waitUntilDone:NO];
		}else
		{
			NSLog(@"error when doemload:%@",error);
			[queue release];
		}
	}
	
	- (void)download_completes:(NSString *)data{
		NSLog(@"call back");
		[self.indicator stopAnimating];
		self.indicator.hidden=YES;
		self.content.text=data;
		[queue release];
	}


##使用GCD后

以上三个方法可以放在一起：

	self.indicator.hidden=NO;
	[self.indicator startAnimating];
	dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0),^{
	//代码块2
	NSURL *url=[NSURL URLWithString:@"http://www.youdao.com"];
	
	NSError *error;
	NSString *data=[NSString stringWithContentsOfURL:url encoding:UTF8StringEncoding error:&error];
	
	if(data!=nil)
	{
		//代码块3
		dispatch_async(dispatch_get_main_queue(),^{
			[self.indicator stopAnimating];
			self.indicator.hidden=YES;
			self.content.text=data;
		});
	}else
	{
		NSLog(@"Error when download:%@",error);
	}
	});
	
代码变得清楚，使用了异步代码。

##使用GCD

bolck的使用在此不用多说。

######系统提供的dispatch方法

为了方便的使用GCD，苹果提供了一些方法方便我们将`block`放在主线程或者后台线程执行或者延后执行。使用例子就像下面一样：

	//后台执行
	dispatch_async(dispatch_get_global_queue(0,0),^{
		//something
	});
	
	
	//主线程执行
	dispatch_async(dispatch_get_main_queue(),^{
		//something
	});
	
	
	//一次性执行
	static dispatch_once_t onceToken;
	dispatch_once(&onceToken,^{
		//code to be executed once
	});
	
	
	//延迟2秒执行
	double delayInSeconds=2.0;
	dispatch_time_t poptime=dispatch_time(DISPATCH_TIME_NOW,
	delayInSeconds *NSEC_PER_SEC);
	dispatch_after (poptime,dispather_get_main_queue(),^{
		//code to be executed on the main queue after delay
	});
	
	
另外，GCD还有一些高级的用法，比如可以让后台两个线程并行执行，然后等两个线程都完成任务后，再汇集执行结果。这个可以用`dispatch_group` `dispatch_group_async` `dispatch_group_notify`来实现，如下：

	dispatch_group_t group=diapatch_group_create();
	dispatch_group_async(group,dispatch_get_global_queue(0,0),^{
		//并行执行的线程
	});
	
	dispatch_group_async(group,dispatch_get_global_queue(0,0),^{
		//并行执行的线程2
	});
	
	dispatch_group_notify(group,dispatch_get_global_queue(0,0),^{
		//汇总结果
	});
	
