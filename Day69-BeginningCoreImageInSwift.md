#Beginning Core Image in Swift

Core Image is a powerful framework that lets you easily apply filters to images. You can get all kinds of effects, such as modifying the vibrance,hue,or exposure. It can use either the CPU or GPU to process the iamge data and is very fast - fast enough to do realtime processing of video frames!

Core Image filters can also be chained together to apply multiple effects to an image or video frame at once. The multiple filters are conbined into a single filter that is applied to the image. This makes it very efficient compared to processing the image through each filter, once at a time.

##Getting Started

Before you get started, let's discuss some of the most important classes in the Core Image Framework:

- `CIContext`:All of the processing of a core image is done in a CIContext. This is somewhat similar to a Core Graphics or OpenGL context.
- `CIImage`:This class hold the image data.It can be created from a UIImage,from an image file,or from pixel data.
- `CIFilter`:The CIFilter class has a dictionary that defines the attributes of the particular that it represents.

You'll be using each of these classes in this project.

##Basic Image Filtering

You're going to get started by simply running your image through a `CIFilter` and displaying it on the screen.Every time you want to apply a CIFilter to an image you need to do four things:

1. **Create a CIImage object**

CIImage has several initialization methods, including :

- `CIImage(contentsOfURL:)`
- `CIImage(data:)`
- `CIImage(CGImage:)`
- `CIImage(bitmapData:bytePerRow:szie:format:colorSpace:)`

You'll most likely be working with `CIImage(contentsOfURL:)`most of the time.

2.**Create a CIContext**

A CIContext can be CPU or GPU based. A CIContext is relatively expensive to initialize so you reuse it rather than create it over and over. You will always need one when outputting the CIImage object.

3.**Create a CIFilter**

When you create the filter, you configure a number of properties on it that depend on the filter you're using.

4.**Get the filter output**

The filter gives you an output image as CIImage - you can convert this to a UIImage using the CIContext, as you'll see below.

Let's see how this works.Add the following code to `ViewController.swift`inside viewDidLoad():

```Swift
//1
let fileURL = NSBoundle.mainBundle().URLForResource("image",withExtension:"png")
//2
let beginImage = CIImage(contentsOfURL: fileURL)
//3
let filter = CIFilter(name:"CISepiaTone")
filter.setValue(beginImage,forKey:kCIInputImageKey)
filter.setValue(0.5,forKey: kCIInputIntensityKey)
//4
let newImage = UIImage(CIImage:filter.outputImage)
self.imageView.image = newImage
```
1. This line creates an NSURL object that holds the path to your image file.
2. Next you create CIImage with the `CIImage(contentsOfURL:)`constructor
3. Next you'll create your CIFilter object. The CIFilter constructor takes the name of the filter, and a dictionary that specifies the keys and the values for that filter.Each filter will have its own unique keys and set of valid values. The `CISepiaTone`filter takes only two values, the `kCIInputImageKey`(a CIImage) and the `kCIInputIntensityKey`,a float value between 0 and 1.Here you give that value 0.5. Most of the filters have default values that will be used if no values are supplied. One exception is the CIImage, this must be provided as there is no default.

4.Getting a CIImage back out of filter is as easy as using the `outputImage` property. Once you have an output CIImage,you will need to convert it into a UIImage.The `UIImage(CIImage:)`constructor create a UIImage from a CIImage. Once you've converted it to a UIImage,you just display it in the image view you added earlier.

Bulid and run the project,and you'll see your iamge filtered by the sepia tone filter.
![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/CI-Sepia-Crop.jpg)

Congratulations, you have successfully used CIImage and CIFilters!

##Putting It Into Context

Before you move forward,there's an optimization that you should know about.

I nnetioned earlier that you need a `CIContext` in order to apply a CIFilter,yet there's no mention os this object in the above example.It turns out that the `UIImage(CIImage:)` constructor does all the work for you.It creates a `CIContext` and uses it to perfrom the work of filtering the iamge. This makes using the Core Image API very easy. 
