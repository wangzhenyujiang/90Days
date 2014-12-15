##饿汉模式

	public class Singleton{
		private final static Singleton INSTANCE=new Singleton();
		
		//Private constructor suppresses
		private Singleton(){}
		
		//default public constructor
		public static Singleton getInstance()
		{
			return INSTANCE;
		}
	}
	
单例模式的思路是：一个类能返回对象一个引用（永远是同一个）和一个获得该实例的方法（必须是静态方法，通常使用getInstance这个名称）；当我们调用这个方法时，如果类持有的引用不为空就返回这个引用，如果类保持的引用为空就创建该类的实例的引用赋予该类保持的引用；同时我们还将该类的`构造函数`定义为私有方法，这样其他处的代码就无法通过调用该类的构造函数来实例化该类的对象，只有通过该类提供的静态方法来得带该类的唯一实例。