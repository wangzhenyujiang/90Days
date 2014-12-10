##桥接模式

简单来说，桥接模式是一个两层的抽象。

桥接模式是用于“把抽象和现实分开，这样它们就能独立变化”。桥接模式使用了封装、聚合，可以用继承将不同的功能拆分为不同的类。

####1、桥接模式的故事

电视和遥控器是一个完美展示两层抽象的例子。你有一个电视接口，还有一个遥控器接的抽象类。我们都知道，将它们中任何一个定义为一个具体类都不是好办法，因为其他厂家会有不同的实现方法。

####2、桥接模式Java示例代码
首先定义电视机接口：ITV

	public interface ITV{
		public void on();
		public void off();
		public void switchChannel(int channel);
	}
	
实现三星的ITV接口：

	public class SamsungTV implements ITV{
		public void on()
		{
			System.out.println("");
		}
		public void off()
		{
			System.out.println("");
		}
		public void switchChannel(int channel)
		{
			System.out.println("");
		}
	}
	
实现索尼的ITV接口：

	public class SonyTV implements ITV
	{
		public void on()
		{
			System.out.println("");
		}
		public void off()
		{
			System.out.println("");
		}
		public void switchChannel(int channel)
		{
			System.out.println("");
		}
	}
	
遥控器要包含对TV的引用：

	public abstract class AbstractRemoteControl{
		private ITV tv;
		
		pulblic AbstractRemoteControl(ITV tv)
		{
		 	this.tv=tv;
		}
		public void turnOn()
		{
			tv.on();
		}
		public void turnOff()
		{
			tv.off();
		}
		
		public void setChannel(int channel){
			tv.switchChannel(channel);
		
		}
	}
	
定义遥控器类：

	public class LogitechRemoteControl extends AbstractRemoteControl {
		public LogitechRemoteControl(ITV tv)
		{
			super(tv);
		}
		
		public void setChannelKeyboard(int channel)
		{
			setChannel(channel);
			System.out.println("Logiech use keyword to set channel.");
		}
	}
	
测试代码：

	public class Main()
	{
		public static void main(String[] args)
		{
			ITV tv=new SonyTV();
			LogitechRemoteControl lrc=new LogitechRemoteControl(tv);
			lrc.setChannelKeyboard(100);
		}
	}
	
总结一下，桥接模式允许两层实现的抽象，上面的电视机和遥控器的例子很好。课件，桥接模式提供了更多灵活性。某个类包含另一个类的引用。


桥接模式应用场景：

- 存在相对并列的子类属性
- 存在概念上得交叉
- 可变性

下面是一个汽车和引擎的例子，汽车有两种卡车和小汽车，这些车都有不同类型的引擎。

汽车抽象类：

	public abstract class Vehicle
	{
		private Engine engine;
		Vehicle(Engine engine)
		{
			this.engine=engine;
		}
		public obstract void setEngine();
		public void setEngine(Engine engine)
		{
			this.engine=engine;
		}
		public Engine getEngine()
		{
			return engine;
		}
	}

下面是汽车抽象类的子类：

	public class Bus extends Vehicle
	{
		public Bus(Engine engine)
		{
			super(engine);
		}
		
		public void  setEngine()
		{
		  System.out.prineln("set bus engine");
		  getEngine().setEngine();
		}
	}
	
下面是货车抽象类的子类：

	public class Tuck extends Vehicle
	{
		public Tuck(Engine engine)
		{
			super(engine);
		}
		
		public void setEngine()
		{
		  System.out.prineln("set Tuck engine");
		  getEngine().setEngine();
		}
	}

下面是引擎类：

	public interface Engine
	{
		public void setEngine(); 
	}
	
实现子类：

	public calss Engine2200 implements Engine
	{
		public void setEngine()
		{
			System.out.println("engine2200");
		}
	}
	
	public calss Engine1500 implements Engine
	{
		public void setEngine()
		{
			System.out.println("engine1500");
		}
	}

桥模式比较适合两个类在不同的纬度进行扩展，比如再加几中不同类型的卡车或者加几种不同类型的引擎然后就可以随意的组装自己想要的车了。
	