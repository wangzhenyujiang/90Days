##适配器模式

`目标接口`，`需要适配的类`，`适配器`

`适配器`继承自`目标接口`，并且调用`需要适配的类`中得方法。也就是说`适配器`依赖于`需要适配的类`。

	/*目标接口*/
	public class Target
	{
		public virtual void request()
		{
			
		}
	}
	
	/*需要适配的类*/
	public class Adaptee 
	{
		public void Special()
		{
		
		}
	}
	
	/*适配器*/
	public class Adapter:Target
	{
		private Adaptee adaptee=new Adaptee();
		
		public override void Request()
		{
			adaptee.Special();
		}
	}
	
应用对象适配器模式实现接口的转换。
	