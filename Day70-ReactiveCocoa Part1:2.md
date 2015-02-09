#ReactiveCocoa Tutoril Part1/2

As metioned in the introduction, ReactiveCocoa provides a standard interface foe handling the disparate stream of *events* that occur within your applicatioin. In ReactiveCocoa terminology these are called signals,and are reprsented by the `RACSignal` class.

Adding the `ReactiveCocoa Framework` to your project and import `<ReactiveCocoa/ReactiveCocoa.h>` to your main Controller.m file.

You aren't going to relpace any of the existing code just yet, for now you're just going to play around a bit. Add the following code to the end of the `ViewDidLoad` method:

```Objective-C
[self.usernameTextField.rac_textSignal subcribeNext:^(id x){
	NSLog(@"%@",x);
}];
```
Bulid and run the application and type some text into the username text field. Keep an eye on the console. You can see that each time you change the text within the text field, the code within the block executes. No target-action, no delegates - just signal and blocks. That's pretty exciting.

ReactiveCocoa siganls (represented by `RACSignal`)send a stream of events to their subscribers. There are three types of events to know: `next` `error` `completed`. A signal may send any number of next events before it terminates after an error, or it completes. 

In this part of tutorial you'll fous on the `next` event。Be sure to read part two when it's avalible to learn about `error` and `completed` events.

`RACSignal` has a number of methods you can use to subscribe to these different event types. Each method takes one or two blocks, with the logic in your block executing when an event occcurs. In this case, you can see that the `subscribeNext:`method was used to supply a block that executes on each `next` event.

The ReactiveCocoa framework uses **category** to add signals to many of the standard UIKit controls so you can add subscriptions to their events ,which is where the `rac_textSignal` property on the text field came from.

But enough with the theory, it's time to start making ReactiveCocoa do some work for you!

ReactiveCocoa framework has a large of operators you can use to manipilate stream of events. For example, assume you're only intereted in a username if it's more than three characters long. You can achieve this by using the `filter` operator. Update the code you added preciously in `viewDidLoad` to the follwing:

```Objective-C
[[self.usernameField.rac_textSignal 
	filter:^BOOL(id value){
		NSString *text=value;
		return text.length >= 3;
	}]
	subscribeNext:^(id x){
		NSLog(@"%@",x);
	}];
```
If your bulid and run, then type some text into the text field, you should find that it only starts logging when the next field length is greater three characters.

**What you've created here is a very simple pipeline. It is the very essence of Reactive Programming, where you express your application's functionality in terms of data flows.**

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/01/FilterPipeline.png)

In the above diagram you can see that the `rac_textSignal` is the initial source of events.The data flows through a `filter` that only allows events to pass if they contain a string with a length that is greater than three. The final step in the piline `subcribeNext:`where your block logs the event value.

At this point it's worth nothing the output of the `filter` operation is also an `RACSginal`. You could arange the code as follows to show the discrete pipeline step:

```Objective-C
RACSignal *usernameSourceSignal = self.usernameTextField.rac_textSignal;

RACSignal *filteredUername = [usernameSourceSignal filter:^BOOL(id value){
	NSString *text=value;
	return text.length >= 3;
}];

[filteredUsername subcribeNext:^(id x){
	NSLog(@"%@",x);
}];
```

**Because each operation on an `RACSignal` also returns an `RACSignal` it's termed a fluent interface. This feature allows you to construct pipelines without the need to reference each step using a local variable**.
