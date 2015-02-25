#Core Image

`Core Image`是一项Mac OS X的技术，利用机器的绘图处理来做影像相关特效。于2004年8月的世界开发者大会(WWDC)中展示。

而`Core Image`有几个特别重要的类别

- `CIContext`:所有Core Image的处理都透过CIContext完成。这有点类似于Core Graphics或OpenGL的Context。
- `CIImage`:该类别可持有图片档案。它可透过UIImage、图片档案或者是像素档案建立起来。
- `CIFilter`:滤镜类别拥有字典(dictionary)，可以指定滤镜效果，例如自然饱和度(vibrance)、色彩反转(color inversion)、剪裁(cropping)...等。

##Step 1.加入`CoreImage.Framework`

##Step 2.编辑`MainStoryboard.storyboard`

加入：

- UIImageView
- UIButton

![](http://lighter.tw/images/CoreImage/2.png) 

##Step 3.编辑ViewController，程式代码如下：

`ViewController.h`的程式代码：

```Objective-C
#import <UIKit/UIKit.h>

@interface ViewController : UIViewCntroller
{
	CIContext *context;
	CIFilter *filter;
	CIImage *inputImage;  //输入的图片
	CIImage *outputImage;  //输出滤镜效果的图片
} 
@property (strong,nonatomic) IBOutlet UIImageView *imageView;
-(IBAction)filter1:(id)sender; //滤镜效果1
-(IBAction)filter2:(id)sender; //滤镜效果2
-(IBAction)filter3:(id)sender; //滤镜效果3
@end

```

`ViewController.m`的程式代码：

```Objective-C
@implementation ViewController
@synthesize imageView;
-(void)viewDidLoad
{
	[super viewDidLoad];
	UIImage *img = [UIImage imageNamed:@"test.jpg"];
	imageView.image = img;
	inputImage = [[CIImage alloc]initWithImage:[UIImage imageNamed:@"test.jpg"]];
}

...
```

接着开始写滤镜效果的按钮方法，这边只介绍一个，程式代码如下：

```Objective-C
-(IBAction)filter1:(id)sender {
    //1
	inputImage = [[CIImage alloc] initWithImage:[UIImage imageNamed:"test.jpg"]];
	//2
	context = [CIContext contextWithOptions:nil];
	//3
	filter = [CIFilter filterWithName:"CIColorPosterize"];
	[filter setValue:inputImage forKey:@"inputImage"];
	[filter setValue:[NSNumber numberWithFloat:3.0] forKey:@"inputLevels"];
	//4
	outputImage = [filter outputImage];
	imageView.image = [UIImage imageWithCGImage:[context createCGImage:outputImage fromRect:outputImage.extent]];
}
```
1. 指定输入的图片
2. `CIContext`指定要使用CPU或是GPU来处理，这里使用预设值就可以，所以指定为`nil`
3. 设置滤镜效果
4. 透过`outputImage`这个方法可以取得滤镜处理效果后的`CIImage`，最后将处理过后的图片指定给`imageView`

##Step 4.编译

编译后就可以看到如下的效果：
![](http://lighter.tw/images/CoreImage/3.png) 
![](http://lighter.tw/images/CoreImage/4.png)
