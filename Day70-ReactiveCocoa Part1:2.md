#ReactiveCocoa Tutoril Part1/2

As metioned in the introduction, ReactiveCocoa provides a standard interface foe handling the disparate stream of *events* that occur within your applicatioin. In ReactiveCocoa terminology these are called signals,and are reprsented by the `RACSignal` class.

Adding the `ReactiveCocoa Framework` to your project and import `<ReactiveCocoa/ReactiveCocoa.h>` to your main Controller.m file.

You aren't going to relpace any of the existing code just yet, for now you're just going to play around a bit. Add the following code to the end of the a `ViewDidLoad` method:

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

##A Little Cast

If you updated your code to split it into the various `RACSginal` compoments, now is the time to revert it back to the fluent syntax:

```Objective-C
[[self.usernameTextField.rac_textSignal 
  filter:^BOOL(id value){
  		NSString *text=value;
  		return text.length>=3;
  }]
  subcribeNext:^(id x){
  		NSLog(@"%@",x);
  }];
```
The implicit cast from `id` to `NSString`, at the indicated location in the code above, is less than elegant. Fortunately,since the value passed to this block is always going to be an `NSString`, you can change the parameter type itself. Update your code as follows:

```Objective-C
[[self.usernameTextFiled.rac_textSignal 
	filter:^BOOL(NSString *text){
		return text.length.=3;
	}]
	subscribeNext:^(id x){
		NSLog(@"%@",x);
	}];
```
Bulid and run to confirm this works just as it did previously.

##What's An Event?

So far this tutorial has described the different event types, but hasn't detailed the structure of these events. **What's interesting is that an event can contain obsolutely anything**.

As an illustration of this point, you're going to add another operation to the pipeline. Update the code you added to `viewDidLoad` as follows:

```Objective-C
[[[self.usernameTextField.rac_textSignal
	map:^id(NSString *text){
		return @(text.length);
	}]
	filter:^BOOL(NSNumber *length){
		return [length integerValue] > 3;
	}]
	subscribeNext:^(id x){
		NSLog(@"%@",x);
	}];
```
If you bulid and run you'll find the app now logs the length of the text instead of the contents.

The newly added map operation tranform the event data using the supplied block. For each `next` event it recives, it run the given block and emits the return value as a `next value`. In the code above, the map takes the `NSString` input and takes its length. which results in an `NSNumber` being returned.

For a syunning graphic depiction of how this works, take a look at this image:
![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/01/FilterAndMapPipeline.png)

**As you can see, all of the steps that follow the `map` operation now receive `NSNumber` instance. You can use the `map` operation to tranform the recived data into anything you like, as long as it's an object.**

##Creating Valid State Signals

The first thing you need to do is create a couple of signals that indicatemwhether the username and password text fields are valid. Add the follwing to the end of `viewDidLoad`.

```Objective-C
RACSignal *validUsernameSignal = 
[self.usernameTextField.rac_textSignal 
	map:^id(NSString *text){
		return @([self isValidUsername:text]);
	}];
	
RACSignal *validPasswordSignal = 
[self.passwordTextField.rac_textSignal 
	map:^id(NSString *text){
		return @([self isValidPasseord:text]);
	}];
```
As you can see, the above code applies a `map` transform to the `rac_textSignal` from each text field. The output is a boolean value boxed as a `NSNumber`.

The next step si to tranform these signals so that they provie a nice background color to text fields. Basicaly you subscribe to this signal and use the result to update the next field background color. One viable option is as follows:

```Objective-C
[[validPasswordSignal 
	map:^id(NSNumber *passwordValid){
		return [passwordValid boolValue] ? [UIColor clearColor] : [UIColor yellowColor];
	}]
	subcribeNext:^(UIColor *color){
		self.passworldTextField.backgroundColor = color;
	}];	
```
Conceptually you're assigning the output of this signal to the `backgroungColor` property of the et field. However, he code above is poor expression of this;it's all backwards!

Fortunately, ReactiveCocoa has a macro that allows you to express this with grace and elegance. Add the following coe directly beneath the two signals you added to `viewDidLoad`:

```Objective-C
RAC(self.passwordTextField,backgroundColor) =
	[validPasswordSignal 
		map:^id(NSNumber  *passwordValid){
			return [passwordValid boolValue] ? [UIColor clearColor] : [UIColor yellowColor];
		}];
		
RAC(self.usernameTextField,backgroundColor) =
	[validUsernameSignal
		map:^id(NSNumber *passwordValid){
			return [passwordValid boolValue] ? [UIColor clearColor] : [UIColor yellowColor];
		}];
```

The RAC macro allow you to assgin the output of signal to property of an object. It takes two arguments,the first is the object that contains the property to set and second is the property name. Each time the signal emits a next event, the calue that passes is assgined to given property. 

Bulid and run the application. You should find that the next fields look highlighted when invalid,and clear when valid.

Visuals are nice ,so here is a way to visualize the current logic. Here you can see two simple pipelines that take the text signals,map them to validity-indicating booleans, and then follow with a second mapping to a `UIColor` which is the part that binds to he background color of the text field.

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/01/TextFieldValidPipeline.png)

##Combining signals

In the current app, the `Sign In` button only works when both the username and pasword text fields have valid input. It's time to do this *reactive-style*.

The current code already has signals that emit a boolean value to indicate if the username and password field are valid. `validUsernameSignal` and `validPasswordSignal`. Your task is to combine these two signals to determine when it is okay to enable the button.

At the end of `viewDidLoad` add the following:

```Objective-C
RACSignal *sigalActiveSignal = 
[RACSignal combineLatest:@[validUsernameSignal,validPasswordSignal]
		   reduce:^id(NSNumber *usernameValid, NSNumber *passwordValid){
						return @([usernameValid boolValue] && [passwordValid boolValue]);
			}];
```

The above code uses the `combineLatest:reduce:` method to combine the latest values emitted by `validUsernameSignal` and `validPasswordSignal` into a shiny new signal. Each time either of the two source signals emits a new value, the reduce block executes, and the value it returns is sent as the next value of the combined signal. 
