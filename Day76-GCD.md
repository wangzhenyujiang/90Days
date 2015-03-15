#GCD

```Objective-C
//创建一个Serial队列
dispatch_queue_t mySerialDispatch = diapatch_queue_create("mySerialDispatch",NULL);

//创建一个Current队列
dispatch_queue_t myCurrentDispatchQueue = diapatch_queue_create("myCurrentDispatchQueue",DISPATCH_QUEUE_CONCURRENT);

//声明队列的调用
dispatch_async(myCurrentDispatchQueue,^{NSLog(@"block on myCourrentDispatchQueue");});

//向队列中添加Tasks
dispatch_async(mySerial,block0);
dispatch_async(mySerial,block1);
```

```Objective-C
dispatch_retain(queue);
dispatch_release(queue);
```
系统自带Dispatch Queue
```Objective-C
dispatch_queue_t mainDispatchQueue = dispatch_get_main_queue();

dispatch_queue_t globalDispatchQueueHigh = dispatch_get_global_queue(DISPATCH_QUEUE_PROPRITY_HIGH,0);

dispatch_queue_t globalDispatchQueueDefault = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0);

dispatch_queue_t globalDispatchQueueLow = dispatch_get_gobal_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0);

dispatch_queue_t globalDispatchQueuebackground = dispatch_get_gobal_queue(DISPATCH_QUEUE_PRIORITY_BACKGROUND,0);
```

改变队列的优先级：

```Objective-C
dispatch_queue_t mySerialDispatch = dispatch_queue_crate("mySerialDispatchQueue",NULL);
dispatch_queue_t globalDispatchBackground = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_BACKGROUND,0);
dispatch_set_target_queue("mySerialDispatchQueue",globalDispatchQueueBackground);
```

dispatch_after:is for setting the timing to start tasks in the queue。当时间到后，dispatch_after方法定义的任务不会立即执行,而是将这个任务加到队列中而已。

```Ovjective-C
disatch_time_t time = dispatch_time(DISPATCH_TIME_NOW,3ull*NSEC_PER_SEC);
dispatch_after(time,dispatch_get_main_queue(),^{
	NSLog(@"waited at least three seconds");
	});
```

毫秒级，微秒级

```Objective-C
dispatch_walltime  //获得准确的时间
dispatch_time  //获得的是相对时间
```

##dispatch_group

```Objective-C
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0);
dispatch_group_t group = dispatch_group_crate();

dispatch_group_async(group,queue,^{NSLog(@"block0");});
dispatch_group_async(group,queue,^{NSLOG(@"block1");});
dispatch_group_async(group,queue,^{NSLog(@"block2");});

dispatch_group_notify(group,dispatch_get_main_queue(),^{NSLog(@"done");});
dispatch_release(group);
```
The execution result:

```Objective-C
block1
block2
block0
done
```
global_dispatch_queue is concurrent dispatch queue.A dispatch group can montior tasks to be finished to be finished no matter in which type of dispatch queues they are.When it detects that all the tasks are finished,the finalizing task is added to the dispatch queue.

而且你可以用`dispatch_group_wait`方法来等待所有任务是否已经完成：

```Objective-C
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0);
dispatch_group_t group = dispatch_group_create();

dispatch_group_async(group,queue,^{NSLog(@"block0");});
dispatch_group_async(group,queue,^{NSLOG(@"block1");});
dispatch_group_async(group,queue,^{NSLog(@"block2");});

dispatch_group_wait(group,DISPATCH_TIME_FOREVER);
dispatch_release(group);
```

`dispatch_group_wait`的第二个参数是`dispatch_time_t`类型，所以我们可以自定义：

```Objective-C
dispatch_time_t = dispatch_time(DISPATCH_TIME_NOW,1ull*NSEC_PER_SEC);

long result = dispatch_group_wait(group,time);
if(result == 0)
{
	/*all tasks that associated with the diapatch group are finished*/
}else
{
	/*some tasks that associated with the dispatch group are still running*/
}
```

##dispatch_sync

##dispatch_apply
就像是dispatch_sync和diapatch_group的集合。

```Objective-C
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0);
dispatch_apply([array count],^(size_t index){
	NSLog(@"%zu:%@",index,[array objectiveAtIndex:index]);
});
```
##dispatch_once

The dispatch_once function is used to ensure that the specified task will be executed only once during the application's lifetime.

应用每次运行只执行一次。