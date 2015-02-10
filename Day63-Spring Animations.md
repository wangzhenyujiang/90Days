#Spring Animations

iOS has a powerful phydics engine bulit into the SDK, and one of the easiest ways to use it is with the new `spring animations`. This type of animation has a timing function like that of an actual spring. You will use this to animate the text field dropping in from the top of the screen, as id it was attached to a spring.

In the `viewController.m`, add a property for the text field to the class extension and update `loadView` to store the reference to the text field. Then start with the text field offscreen:

```Objective-C
@interface viewController() <UITextFieldDelegate>
@property (nonatomic,weak)UITextField *textField;
@end

@implementation viewController
-(void)viewLoad
{
	CGRect frame=[UIScreen main].bounds;
	UIView *backgroundView=[[UIView alloc]initWithFrame:frame];
	
	CGRect textField=CGRectMake(40,70,240,30);
	UITextField *textField=[[UITextField alloc]initWithFrame:textFieldRect];
	[backgroundView addSubView:textField];
	
	self.textField=textField;
	self.view=backgroundView;
}
@end
```

```Objective-C
-(void)viewDidApper:(BOOL)animated
{
	[super viewDidappear:animated];
	
	[UIView viewWithDuration:2.0
						   dalay:0.0
	   usingSpringWithDamping:0.25
	    initialSpringVelocity:0.0
	                  options:0
	               animations:^{
	   					CGRect frame = CGrectMake(40,70,240,30);
	   					self.textField.frame=frame;
	                  }
	               completion:nil];
}
```

Now individual components of this method are relatively straightforward:

- `duration`: Total time the animation should last
- `delay`: How long until the animation should begin
- `springDamping`: A number between 0 and 1. The closer to 0,the more the animation oscillates
- `springVelocity`: The relative velocity of the view when the animation is to begin. You will almost always pass in 0 for this.
- `options`: `UIViewAnimationOptions`,just like with the other animations.
- `animations`: A block of change to animate on one or more views.
- `completion`: A block to run when the animation is finished. 
