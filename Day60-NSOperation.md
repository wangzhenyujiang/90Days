#NSOperation

The `NSOperation` class has a farily easy and short declaration. To create a customized operation, follow these steps:

- Subclass NSOperation
- Overide "main"
- Create an "autoreleasepool" in "main"
- Put your code within the "autoreleasepool"

The reason youe should create your own autorelease pool is that you do not have access to the autorelease pool of the main thread, so you should create your own. Here is an example:

```Objective-C
#import <Foundation/Foundation.h>

@interface MyOperation: NSOperation
@end
```

```Objecitve-C
@implementation MyOperation

-(void)main
{
	@autoreleasepool{
		for(int i = 0; i < 1000 ; i++){
			NSLog(@"%f",sqrt(i));
		}
	}
}
@end
```

In threaded operation, you never know exacty when the operation is going to start, and how long it will take to finish. Most of the time you don't want to perform an operation in th background if the user has scrolled away or has left a page - there's no reason to perform the operation .The key is to check for `isCancelled` property of Operation class frequently. For Example, in the imaginary sample code above, you would do this:

```Objective-C
@interface MyOperation : Operation
@end

@implementation MyOperation

-(void)main
{
	@autoreleasepool
	{
		for(int i = 0; i < 1000 ; i++)
		{
			if(self.isCancelled)
				break;
			NSLog(@"%f",sqrt(i));
		}
	}
}
```

To cancle an operation, you call the NSOperation's `cancel` method ,as shown:

```Objectibe-C
// In your controller class, you create the NSOperation
//Create the operation
MyOperation *operation = [[MyOperation alloc]init];
.
.
.//Cancel it
[operation cancel];
```

NSOperation calss has a few other methods an properties:

- `Start:`Normally,you will not override this method. Overriding `Start` requires a more complex implementation, and you have to take a care of properties such as isExecuting, isFinished, isConcurrent, and isReady. When you add an operation to a `Queue`, the `Queue` will call `Start` on the operation and that will result in some perparation and the subsequent execytion of "main". **If you call `Start` on an intance of NSOperation, without adding it to a queue, the operation will run in a main ru loop** .
- `Dependency:`You can make an operatin dependent on other operations. Any operation can be dependent on any number of operations. When you make operation A dependent on operation B, even though you call `start` on operation A, it will not start unless operation B `isFinished` is true. For example:

```Objective-C
MyDownloadOperatoin *operationA = [[MyOperation alloc]init];
MyFilterOperation *operationB = [[MyOperation alloc]init];

[operationB addDependency:operationA];
```
To remove dependencues:

```Objective-C
[operationB removeDependency:operationA];
```

- `Priority:`sometimes the operation you wish to run in the background is not crucial and can be performed at a lower priority. You set the priority of an operation by using `setQueuePriority:`.

```Objective-C
[operation setQueuePriority:NSOperationQueuePriorityVeryLow];
```
Other options for thread priority are : `NSOperationQueuePriorityLow` `NSOperationQueuePriorityNormal` `NSOperationQueuePriorityHigh` `NSOperationQueuePriorityVeryHigh`

- `Completion block:` another useful method in NSOperation class is `setCompletionBlock:`. If there is something that you want to do once the operation has been completed, you can put it in a block and pass it int this method. Note that there is no guarantee the block will be executed on the main thread.

####Some additional notes on working with operations:

- If you need to pass in some values and pointer to an operation,it is good practice to create your own designated initializer:

```Objective-C
@interface MyOperation : NSOperation

-(id)initWithNumber:(NSNumber *)start string:(NSString *)string;

@end
```

- If your operation is going to have a return value or object, it's a good practice to declare delegate methods. Usually you want to call back to the delegate method on the main thread. You make the compiler happy, you need to cast the delegate to NSObject:

```Objective-C
[(NSObject *)self.delegate performSelectorOnMainThread:@selector(delegateMethod:) withObject:object waitUntilDone:NO];
```

- You cannot enqueue an operation again. Once it is added to a queue, you should give up ownership. If you want to use the same operation class again, you need to create a new instance.
- A finished operation cannot be restarted.
- If you cancel an operatin, it will not happen instantly. It will happen at some pint in the future when someone explicitly checks for `isCanceled == YES`.otherwise, the operation will run until it is done.
- Whether an operation finishes successfully, or is cancelled,the value of isFinished will always be set to YES. Therefore never assume that `isFinished == YES`means everything went well - partcularly, if there are dependencies in your code .



