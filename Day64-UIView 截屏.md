#UIView 截屏

>`UIGraphicsBeginImageContextWithOptions`函数的使用


##`UIGraphicsBeginImageContextWithOptions`函数原型：

```Objective-C
UIGraphicsBeginImageContextWithOptions(GSize size,BOOL opaque,float scale);
```

- size:为新建的位图上下文的大小
- opaque:透明开关，如果图形完全不透明，设置为YES以优化位图储存
- scale:缩放因子

这里需要判断一下`UIGrapbicsBeginImageContextWithOptions`是否为NULL，以为它是在iOS4.0之后才加入的。如果JPEG格式就显示YES，如果PNG就设置NO。第三个参数iPhone4是2.0其他时1.0。

以我项目的代码为例：

```Objective-C
if(UIGraphiceBeginImageContextWithOptions != NULL)
{
	UIGraphicsBehinImageContextWithOptions(scrollView.contentSize,NO,2.0);
}else{
	UIGraphicsBeginImageContext(scrollView.contextSize);
}

[scrollView.layer redeerInContext:UIGraphicsGetCurrentContext()];
image= UIGraphicsGetImageFromCurrentImageContext;
//保存截图
UIGraphicsEndImageContext();
//返回截图
```