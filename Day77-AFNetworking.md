#AFNetworking Usage

##`Get` Request

```Objective-C
AFHTTPRequestOperation *manager = [AFHTTPRequestOperation manager];
[manager GET:@"http://example.com/resources.json" parameters:nil success:^{AFHTTPRequestOperation *operation, id responseObject}{
	NSLog(@"JSON:%@",responseObject);
}];
```

##`POST` URL-Form-Encoded Request

```Objective-C
AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
[manager POST:@"http://example.com/resources.json" parameters:parameters success^(AFHTTPRequestOperatoin *operation,id resopnseObjective){
	NSLog(@"JSON:%@",responseObjective);
}failure:^(AFHTTPRequestOperation *operation,NSError *error){
	NSLog(@"Error:%@",[error description]);
}];
```

##Category to UIImageView

```Objective-C
NSURL *url = [NSURL URLWithString:daysWeather.wearherIconURL];
NSURLRequest *request = [NSURLRequest requesWithURL:url];
UIImage *placeholderImage = [UIImage imageNamed:@"placeholder"];

__weak UITableViewCell *weakCell = cell;

[cell.imageView setImageWithURLRequest:request
					  placeholderImage:placeholderImage
                               success:^(NSURLRequest *request,NSURLPURLRespon *response,UIImage *image){
	weakCell.imageView.image = image;
	[weakCell setNeedsLayout];
}failure:nil];
```

##Operation Json

```Objective-C

    NSString *string = [NSString stringWithFormat:@"%@weather.php?format=json", BaseURLString];
    NSURL *url = [NSURL URLWithString:string];
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
 
    AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];
    operation.responseSerializer = [AFJSONResponseSerializer serializer];
 
    [operation setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation, id responseObject) {

        self.weather = (NSDictionary *)responseObject;
        self.title = @"JSON Retrieved";
        [self.tableView reloadData];
 
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {

        UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"Error Retrieving Weather"
                                                            message:[error localizedDescription]
                                                           delegate:nil
                                                  cancelButtonTitle:@"Ok"
                                                  otherButtonTitles:nil];
        [alertView show];
    }];
 
    [operation start];
```

##Operation Property Lists

```Objective-C
 NSString *string = [NSString stringWithFormat:@"%@weather.php?format=plist", BaseURLString];
    NSURL *url = [NSURL URLWithString:string];
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
 
    AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];
 
    // Make sure to set the responseSerializer correctly
    operation.responseSerializer = [AFPropertyListResponseSerializer serializer];
 
    [operation setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation, id responseObject) {
 
        self.weather = (NSDictionary *)responseObject;
        self.title = @"PLIST Retrieved";
        [self.tableView reloadData];
 
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
 
        UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"Error Retrieving Weather"
                                                            message:[error localizedDescription]
                                                           delegate:nil
                                                  cancelButtonTitle:@"Ok"
                                                  otherButtonTitles:nil];
        [alertView show];
    }];
 
    [operation start];
```

##NetWorkActivityIndicator

```Objective-C
[AFNetWorkActivityIndicatorManager shareManager].enabled = YES;
```
##Downloading Images

```Objective-C
NSURL *url = [NSURL URLWithString:@"https://www.raywenderlich.com/wp-content/uploads/2014/01/sunny-background.png"];
NSURLRequest *request = [NSURLRequest requestWithURL:url];

AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc]initwithReuest:request];
operation.responseSerializer = [AFImageResponseSerializer serializer];

[operation setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation,id responseObject){
	self.backgroundImageView.image = responseObjective;
	[self saveImage:responseObject withFileName:@"background.png"];
}failure:^(AFHTTPRequestOperation *operation,NSError *error){
	NSLog(@"Error:%@",error);
}];

[operation start];
```