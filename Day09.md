1）C++子类对象赋值给父类对象的时候会发生什么？内存是怎么分配的？

##面向对象复习
####第四章 类和方法

- 接口：不提供实现，接口定义新类型，可以声明变量。

- c++和java嵌套类和内部类的区别。

####第五章 消息、实例和初始化

- 静态类型语言（java,C++，C#,Pascal）类型和变量联系在一起，编译时做出内存分配决定。动态类型语言（smatlltalk,python）数据和数值联系在一起。

- 对象数组的创建，C++结合对象适用缺省的构造函数来初始化。Java：new仅创建数组。数组包含的对象必须独立创建。

- 内存回收：使用new创建堆内存。C++要delete，Java垃圾回收机制（通常要在内存快耗尽的时候工作）。

- 静态存储分配要求在编译时能知道所有变量的存储要求,栈式存储分配要求在过程的⼊入⼜⼝口 处必须知道所有的存储要求,⽽而堆式存储分配则专门负责在编译时或运⾏行时模块⼊入⼜⼝口处都 ⽆无法确定存储要求的数据结构的内存分配,⽐比 如可变长度串和对象实例。堆由⼤大⽚片的可利⽤用块或空闲块组成,堆中的内存可以按照任意顺序分配和释放。

- const和final的区别：const常量不允许改变，final仅断言相关变量不会被赋予新值，并不能阻止在对象内部对变量进行的修改。

- 元类



####第六章 继承

- 替换原则：如果有A和B两个类，类B是类A的子类，那么在任何情况下都可以使用类B来替换类A。

- 特化子类化：这种形式支持替换原则，这种方式是使用继承最常用的思维方式。

- 规范子类化：这种子类化不是对已有类型的改进，而是实现了父类中未完成的抽象规范定义。规范子类化的使用场合是：父类仅仅对行为进行了定义，却没有对其进行实现，而由子类来实现这些行为。

- 结合子类化：这种模式使用的场合是，需要一个子类，可以表示两个或者更多的父类的特征的结合。比如一个助教要有两种身份，分别是老师和学生。可以用继承与两个或者多个父类的能力称为多重继承。



####第七章 静态行为和动态行为
- C++使用指针或者引用；相关方法声明为vritual；才可以实现多态消息的传递。

######运行时类型决定

######静态方法绑定和动态方法绑定

####第八章 替换的本质

- 当子类实例负值给父类实例的时候，父类实例不能调用子类实例中的方法。

- C++中的深复制使用拷贝构造函数，Java中的深复制是改写clone方法。（默认的克隆方法为浅克隆，只克隆对象的非引用类型成员）

- 最小静态空间分配：只分配该分配的空间，子类赋值给父类时将会缺省的切割掉子类多余的部分。

- 动态内存分配：在栈中只有子类和父类的指针类型变量，具体的数据内存在堆中分配，指针变量的大小都是相等的，所以将指向子类的指针指向父类的时候就不会出错了。



######内存布局

######复制和克隆

####第十一章 重载

**函数签名：**它是关于函数参数类型、参数顺序和返回值类型的描述。




####第十三章 多态变量

**多态变量**是指可以引用多种对象类型的变量。

- 纯多态 例子：java语言编写的StringBufffer类中得append方法。在这个方法的参数名声明为Object类型。因此可以表示任何对象类型。例子：toString()方法被延迟实现。toString在子类中得以重定义。toString()方法的各种不同版本产生不同的结果。

####十四章 泛型

- 思考 在不允许改变原有类的情况下，如何让将来自两个或者更多个不同类的元素结合起来。

第一步 ：定义一个公共的父类，具有共同行为

第二步 ：使用模板，创建一个fruit adapter类，将以apple或者orange为参数，同时符合水果的定义。

第三步 ：适用模板函数简化适配器创建过程。

####十五章 框架

####面向对象的设计原则

######提高面向对象设计复用性的设计原则
######开闭原则OCP
**软件组成实体应该是对扩展开放的但是对修改是关闭的。**

怎样遵循OCP?

- 编写一个shape抽象类
- 这个类仅有一个抽象方法draw()
- 所有形状都将从这个类派生
- 当绘制一种新的形状，只需要增加一个新的shape类的派生类。而函数并不改变。

######里氏替换原则LSP
**使用指向基类的引用的函数，必须能够在不知道具体派生类对象类型的情况下使用它们，也就是凡是父类适用的地方子类也适用**
######依赖倒转原则
**抽象不应该依赖于细节，细节应当依赖于抽象**
######接口隔离原则
######组合隔离原则
**优先使用（对象）组合，而非（类）继承**

**组合优点：**

- 容器类仅能通过被包含对象的接口来对其进行访问。
- 黑盒复用，因为被包含对象的内部细节对外是不可见的。
- 封装性好。
- 实现上的相互依赖性比较小。
- 每一个类只专注于一项任务。
- 通过获取指向其它的具有相同类型的对象引用，可以在运行期间动态的定义组合。

**组合缺点：**

- 导致系统中对象过多。

**继承优点**

- 容易进行新的实现，因为其大多数可以继承而来。

- 易于修改或扩展那些被复用的实现。

**继承缺点**

- 破坏了封装性，因为这会将父类的实现细节暴露给子类。
- “白盒”复用，因为父类的内部细节对于子类而言通常是可见的。
- 当弗雷的实现更改时，子类也不得不随之更改。
- 从父类继承来的实现将不能在运行期间改变。


######迪米特法则

**又称最少知识原则，一个对象应该对其他对象尽可能少的了解。**

######单一职责原则

######接口隔离原则

**使用多个专门的接口比使用单一的总接口好。一个类对另一个类的依赖应该建立在最小的接口上。**

####模式

######简单工厂模式

简单工厂包含三个角色：工厂、抽象产品、具体产品

下面看一个小例子：

先定义飞机接口类：

	public interface IPlane
	{
		void name();
	}

再得到Product：

波音：
	
	public class Boeing777 implements IPlane{
		private String name="Boeing777";	
		public void name()
		{
			System.out.println("My name is :"+name);
		}
	}	
	
空客：

	public class Boeing737 implements IPlane
	{
		private String name="Boeing737";
		public void name()
		{
			System.out.println("My name is :"+name);
		}
	}
	
再看飞机工厂：

Create:

	public class PlaneFactory
	{
		public static getPlane(String name) throw Exception
		{
			if(name.equalsIgnoreCase("boeing777"))
			{
				return new Boeing777();
			}else if(name.equalsIgnoreCase("boeing737"))
			{
				return new Boeing737();
			}else
			{
				throw new Exception("The type $type don't exists" ); 
			}
		}
	}

当我们在客户端想要得到飞机的时候：

	public class PlaneTest
	{
		public static void main(String[] args)theows Exception
		{
			IPlane boeing777 = PlaneFactory.getPlane("boeing777");
			boeing777.name();
			IPlane boeing737=PlaneFactory.getPlane("airbus");
			boeing737.name();
		}
	}

以上就是一个小小的用了简单工厂模式的代码。
######工厂方法模式

把工厂类修改成抽象类：

	public abstract class PlaneFactory
	{
		public void letPlaneFly(String name)
		{
			IPlane iPlane=getPlane(name);
			iPlane.fly();
		}
		abstact IPlane getPlane(String name);
	}//不但获得了飞机类，还能让它飞。
	
具体工厂类，有boeing工厂和aribus工厂

	public class AribusFactory extends PlaneFactory
	{
		IPlane getPlane(String name)
		{
			if (name.equalsIgnoreCase("airbus320"))
			{  
            	return new Airbus320();  
        	} else if(name.equalsIgnoreCase("airbus380"))
        	{  
           	 	return new Airbus380();  
       		}else 
       		{  
            	return null;  
        	}  
		}
	}
	
	public class BoeingPlaneFactory extends PlaneFactory
	{
		IPlane getPlane(String name) 
			{
				if (name.equalsIgnoreCase("boeing777"))
				{
					return new Boeing777();
				} else if
				(name.equalsIgnoreCase("boeing737"))
				{
					return new Boeing737();
				}else
				{
					return null;
				}		
			}

	}

产品抽象类：plane

	public abstract class IPlane
	{
		abstract void name();
		public void fly()
		{
			name();
			System.out.println("i fly in the sky");
		}
	}

分别继承获得的四个飞机类：

	public class Boeing777 extends IPlane
	{
		public void name()
		{
			System.out.println("i am boeing777");
		}
	}
	
	public class Boeing737 extends IPlane
	 {
		public void name() 
		{
		System.out.println("i am boeing 737");
		}

	}
	
	public class Airbus320 extends IPlane
	 {
		public void name() 
		{
		System.out.println("i am Airbus320");
		}

	}
	
	public class Airbus380 extends IPlane 
	{
		public void name() 
		{
			System.out.println("i am Airbus380");
		}

	}

测试类：

	public static void main(String[] args) throws Exception {
		PlaneFactory airBus = new AirbusFactory();
		airBus.letPlaneFly("airbus320");
		airBus.letPlaneFly("airbus380");
		
		PlaneFactory boeing = new BoeingPlaneFactory();
		boeing.letPlaneFly("boeing777");
		boeing.letPlaneFly("boeing737");
		
	}
	
总结，工厂模式，有抽象的工厂类，有具体工厂类，有抽象产品类，有具体产品类。
	
######抽象工厂模式
	 

         
抽象工厂模式和工厂方法模式的区别:

工厂方法模式：

- 一个抽象产品类，可以派生出多个具体产品类。

- 一个抽象工厂类可以派生出多个具体的工厂类。

- 每个具体工厂类只能创建一个具体产品类的实例。

抽象工厂模式：

- 多个抽象产品类，每个抽象产品类可以派生出具体产品类。

- 一个抽象工厂类，可以派生出多个具体工厂类。

- 每个具体工厂类可以创建多个具体产品类的实例。

区别：

- 工厂方法模式只有一个抽象产品类，而抽象工厂模式有多个。
- 工厂方法模式的具体工厂类只能创建一个产品类的实例，而抽象工厂模式可以创建多个。


######策略模式

策略模式包括：抽象策略角色、具体策略角色，环境角色。

环境角色：包含一个抽象策略角色的引用，通过set方法或构造函数完成策略对象的赋值，并提供给客户端一个策略调用的方法。

下面写一个超市打折促销的例子来具体说明：

价格计算抽象策略类：

	public interface ICalculationPriceStrategy
	{
		public double calculationPrice(double price);
	}

普通会员价格计算的策略类实现类：

	public class OrdinaryMemberStrategy implements ICalculationPriceStrategy
	{
		public double calculationPrice(double price)
		{
			System.out.println("--采用普通会员价格计算策略")；
			return price;
		}
	}
	
中级会员价格计算的策略实现类：

	public class IntermediateMemberStrategy implements ICalculationPriceStrategy
	{
		public double calculationPrice(double price)
		{
			System.out.println("--采用中级会员策略");
		}
	}
	
高级会员价格计算策略实现类：

	public class SeniorMemberStrategy implements ICalculationPriceStrategy
	{
		public double calculationPrice(double price)
		{
			System.out.println("--采用高级会员策略");
		}
	}
	
环境角色：

	public class CalcPriceContext{
		private ICalculationPriceStrategy calcPriceStrategy;
		
		public CalcPriceContext(ICalculationPriceStrategy calcPriceStrategy)
		{
			this.calcPriceStrategy=calcPriceStrategy;
		}
		
		public double calculationPrice(double price)
		{
			double finalPrice=0;
			if(calcPriceStrategy!=null)
			{
	finalPrice=calcPriceStrategy.calculationPrice(price);
				
			}
			return finalPrice;
		}
	}

客户端测试类:

	public class TestMian()
	{
		public static void main(String[] args)
		{
			double price=600;
			doubel finalPrice=0;
			CalcPriceContext context=null;
			context=new CalcPriceContext(new OrdinaryMemberStrategy());
			finalPrice=context.calculation(price);
			System.out.println("普通会员");
			context=new CalcPriceContext(new IntermediateMemberStrategy());
			finalPrice=context.calculation(price);
			System.out.println("中级会员");
			context=new CalcPriceContext(new SeniorMemberStrategy());
			finalPrice=context.calculation(price);
			System.out.println("高级会员");
		}
	}
	
	
总结：

策略模式首先要有一个抽象策略角色，其次是继承或者实现接口的具体策略类。然后有一个管理具体策略类的环境角色，环境角色中有一个抽象策略角色，用来接受传进来的具体策略类。

策略模式的优点如下：

- 策略模式提供了管理相关的算法族的方法。策略类的等级结构定义了一个算法或行为族。恰当使用继承可以把公共的代码移到父类里面，从而避免代码重复。
- 使用策略模式可以避免使用多重条件(if-else)语句。多重条件语句不易维护，它把采取哪一种算法或采取哪一种行为的逻辑与算法或行为的逻辑混合在一起，统统列在一个多重条件语句里面不利于代码的扩展。
- 修改具体策略不影响客户端的调用。

策略模式的缺点如下：

- 客户端必须知道所有的策略类，并自行决定使用哪一个策略类。这就意味着客户端必须理解这些算法的区别，以便适时选择恰当的算法类。换言之，策略模式只适用于客户端知道算法或行为的情况。
- 由于策略模式把每个具体的策略实现都单独封装成为类，所以会增加系统需要维护的类的数量。
- 在基本的策略模式中，选择所用具体实现的职责由客户端对象承担，并转给策略模式的Context对象。这本身没有解除客户端需要选择判断的压力，而策略 模式与简单工厂模式结合后，选择具体实现的职责也可以由Context来承担，这就最大化的减轻了客户端的压力。