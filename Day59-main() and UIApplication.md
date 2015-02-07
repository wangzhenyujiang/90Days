# main() and UIApplication

A `C` application begins by executing a `main` function. An Objective-C application is no different, but you have not seen `main()` in any of your iOS application.Let's take a look now.

Open `main.m` in any project. It looks like this:

	int main()
	{
		@autoreleasepool
		{
			return UIApplicationMain(argc,argv,nil,NSStringFromClass([ProjectNameDelegate class]));
		}
	}
	
The function `UIApplicationMain` creates an instance od a class called `UIApplication`. For every application, there is a single `UIApplication` instance. This object is responsible for maintaining the run loop. Once the application object id create,its run loop essentially becomes an infinite loop: the executing thread will never return to `main()`.

Another thing the function `UIApplicationMain` does is create an instance of the class that serve as the `UIApplication`'s delegate. Notice that the final argument to the `UIApplicationMain` function is an `NSString` that is the name of the delegate's class. So this function will create an instance os `ProjectNameDelegate` and set it as the `delegate` of the `UIAppliaction` object.

The first event added to the run loop in every application is a special "kick-off" event that triggrts the application to send a message to its `delegate`. This maeeage is `application:didFinishLaunchingWithOptions:`. You implemented this method in `ProjectNameDelegate` to create the window an the controller objects used in this application.

