##图片抖动（左右摇晃）

最近要做一个摇一摇的功能，为了更人性化一些，要实现图片随着手机的摇动而抖动（左右摇晃），一下是代码：

首先在工程中要导入<QuartzCore/QuartzCore.h>这个框架。然后创建一个ImageView对象，然后当检测到手机产生摇动后，调用一下代码：

	CABasicAnimation *shake = [CABasicAnimation animationWithKeyPath:@"transform.rotation.z"];
	
	shake.formValue=[NSNumber numberWithFloat:0.2];
	
	shake.toValue=[NSNumber numberWithFloat:0.2];//double值可修改，为摆动幅度
	
	shake.duration=0.1;//摆动一次耗时
	
	shake.autoreverses=YES;
	
	shake.repeatCount=5;//摆动次数
	
	[self.logobg.layer addAnimation:shake forKey:@"shakeAnimation"];