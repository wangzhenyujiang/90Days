##装饰模式

>动态的给一个对象添加一些额外的职责。就增加功能而言，装饰模式比生成子类更灵活。

下面是用装饰模式实现一个对某个手机的GPS和蓝牙的功能扩展：

	public abstract class AbstractCellPhone
	{
		public abstract String CalNumber();
		public abstract String SendMessage();
		
	}
下面我们要实现具体的手机类，Nokia和Moto的手机类：

	public class NokiaPhone:public AbstractCellPhone
	{
		public override String CallNumber()
		{
			return "NokiaPhone call someBody";
		}
		public override String SendMessage()
		{
			return "NokiaPhone send a Message to someBody";
		}
	}
	
	public class MotoPhone:public AbstractCellPhone
	{
		public override String CallNumber()
		{
			return "MotoPhone call someBody";
		}
		public override String SendMessage()
		{
			return "MotoPhone send a Message to someBody";
		}
	}
	
接下来我们需要一个装饰接口或者抽象类，实现代码如下：

	public abstract class Decorator:AbstractCellPhone
	{
		AbstractCellPhone _phone;
		
		public Decorator(AbstractCellPhone phone)
		{
			_phone=phone;
		}
		
		public override String CallNumber()
		{
			return _phone.CallMunber();
		}
		public override String SendMessage()
		{
			return _phone.SendMessage();
		}
	}
	
蓝牙装饰类：

	public class DecoratorBlueTooth:Decorator
	{
		public DecoratorBlueTooth(AbstractCellPhone phone):base(phone)
		{
		
		}
		
		public override String CallNumber()
		{
			return base.CallNumber()+"with buleTooth";
		}
		
		public ovrride String SendMessage()
		{
			return base.SendMessage()+"with buleTooth";
		}
	}
	
	public class DecoratorGPS:Decorator
	{
		public DecoratorGPS(AbstractCellPhone phone):base(phone)
		{
		
		}
		
		public override String CallNumber()
		{
			return base.CallNumber()+"with GPS";
		}
		
		public ovrride String SendMessage()
		{
			return base.SendMessage()+"with GPS";
		}
	}

	
装饰类也就是说，你把一个东西丢进来，我给你装饰一下，可以用多个装饰类修饰一个具体的产品。

- 通过组合而非继承的手法，Aecorator模式实现了在运行时动态的扩展对象功能的能力，而且可以根据需要扩展多少功能。
- Component类在Decorator模式中充当抽象接口的角色，不应该去实现具体的行为。
- Decorator类在接口上表现为is-a Compoent的继承关系，即Decorator类继承了Component类所有具有的接口。但事实上又表现出了has-a Component的组合关系，即Decorator又使用了另外一个Component类。我们可以使用一个或者多个Decorator对象来装饰一个Component对象，且装饰后的对象仍然是一个Component对象。
- 在这里我想谈一下我的理解：当我们实例化一个Component对象后，要给这个对象扩展功能，这时我们把这个Component对象当作参数传给Decorator的子类的构造函数——也就是扩展方法的功能类。
- 装饰模式解决的是主体类在多个方向上的功能扩展功能。

下面是一个装饰模式咖啡的例子，咖啡有可能增加多种添加剂，比如加了牛奶的，加了牛奶和红豆的等等。

先是饮料的抽象类:

	public interface Beverge
	{
		public String getDesription();
		public double getPrice();
		
	}
	
下面是被装饰的咖啡具体类：

	public class coffce1 implements Beverge
	{
		private String description="first coffce";
		
		public String getDesription()
		{
			return description;
		}
		public double getPrice()
		{
			return 50;
		}
	}
	
	public class coffce2 implements Beverge
	{
		private String description="second coffce";
		
		public String getDescripton()
		{
			return description;
		}
		public double getPrice()
		{
			return 48;
		}
	}
	
装饰器类:

	public class Decorator implements Beverage
	{
		private String description="Decorator";
		public String getDescription()
		{
			return description;
		}
		public double getPrice()
		{
			return 0;//价格由子类决定
		}
	}
	
具体装饰类：

	public class Milk extends Decorator
	{
		private String description="加了牛奶";
		private Beverge beverge=null;
		
		Milk(Beverge beverge)
		{
			this.beverge=beverge;
		}
		
		public String getDescription()
		{
			return beverge.getDescription+description;
		}
		
		public double getPrice()
		{
			return beverge.getPrice()+20;//20表示牛奶价格
		}
	}
	
	public class Mocha extends Decorator
	{
		private String description="加了摩卡";
		private Beverge beverge=null;
		
		Milk(Beverge beverge)
		{
			this.beverge=beverge;
		}
		
		public String getDescription()
		{
			return beverge.getDescription+description;
		}
		
		public double getPrice()
		{
			return beverge.getPrice()+40;//40表示摩卡价格
		}
	}
	
	
装饰：在之前的基础上累加，扩展功能，装饰器类，具体的被装饰类。严格按照类图。重点是一个具体产品的纵向功能扩展，桥接模式是多个并列关系的产品的横向上的扩展。

装饰模式的优点：

- 装饰模式与继承关系的目的都是要扩展对象的功能，但是装饰模式可以提供比继承更多的灵活。
- 通过使用不同的具体装饰类以及这些修饰类的排列组合。

装饰模式的缺点：

- 使用装饰模式会产生比使用继承关系更多的对象。更多的对象会使得查错变得困难，特别是这些对象看上去都很相像。