#Downloading Images

Here is the final AFNetworking trick for this tutuorial: `AFHTTPRequestOperation` can also handle image requests by setting its `responseSerializer` to an instance of `AFImageReaponseSerializer`.

```Objective-C
NSURL *url = [NSURL URLWithString:@"@"http://www.raywenderlich.com/wp-content/uploads/2014/01/sunny-background.png"];
NSURLRequest *request=[NSURLRequest requestWithURL:url];

AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];
operation.responseSerializer=[AFImageResponseSerializer serializer];

[operation setCompletionBlockWithSucess:^(AFHTTPRequestOperation *operation,id responseObject){
		self.backgroundImageView,image=responseObject;
		[self saveImage:responseObject withFilename:@"background.png"];
	}failure:^(AFHTTPRequestOperation *operation,Error *error){
		NSLog(@"Error:%@",error)
	}];

[operation start];
``` 
This method initiates and handle downloading the new background. On completion, it return the ful image requested.
