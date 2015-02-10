#Keyframe Animations

The animations you have added so far have been basic animations; they animate from one value to another value. If you want to animate a view's properties through more than two values, you use a `keyframe animation`. A keyframe animation can be made up of any number of individual keyframes. You can think of keyframe animations as multiple basic animaitons going back to back.

Keyframe animations are set up similarly to basic animations, but each keyframe is added apearately.To create a keyframe animation, use the :

```Objective-C
animateKeyframeWithDuration: delay: options:animations:
```
class method on `UIView`, and add keyframes in the animation block using the:

```Objective-C
addKeyframeWithRelativeStartTime: relativeDuration: animations:
```
class method.

```Objective-C
[UIView animateWithDuration:0.5
						  delay:0.0
						options:UIViewAnimationOptionCureEaseIn
					 animations:^{
					 	//animation code
					 }
					 completion:^(BOOL finished){
					 	//completion code
					 }];
```

```Objective-C
[UIView animateKeyframesWithDuration:1.0 delay:0.0 options:0 
	animations:^{
		[UIView addKeyframeWithRelativeStartTime:0 relativeDuratior:0.8 animation:^{
			//animation code
		}];
		[UIView addKeyframeWithRelativeStartTime:0.8 relativeDuratior:0.8 animation:^{
			//animation code
		}];
		}
	completion:^(BOOL finished){
		//completion code
		}];
```

Keyframe animations are created using `animationKeyframeWithDuration: delay: option: animation: completion:`. The parameters are all the same as with the basic animation except that the option are of type `UIViewKeyframeAnimationOptions` instead of `UIViewAnimationOptions`. The duration passed into this method is the duration of the entire animation.

Individual keyframes are assed using `addKeyframeWithRelativeStartTime: relativeDuration: animations:`.

##Preference
[需要翻墙](http://captechconsulting.com/blog/tyler-tillage/ios-7-tutorial-series-custom-navigation-transitions-more)