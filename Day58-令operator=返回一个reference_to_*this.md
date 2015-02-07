#令operator=返回一个reference to *this

关于赋值，有趣的是你可以把它们写成连锁形式：
	
		int x, y, z;
		x = y = z = 15;  //赋值连锁形式
同样有趣的是，赋值采用左右结合律，所以上述连锁赋值被解析为：
		
		x = (y = (z = 15));
这里15先背赋值给`z`，然后其结果(更新之后的`z`)再被赋值给y，然后其结果(更新后的`y`)再被赋值给`x`。

为了实现“连锁赋值”，赋值操作符必须返回一个reference指向操作符的左侧实参。这是你为classes实现赋值操作符时应该遵循的协议：
		
		class Widget{
		public:
			...
			Widget& operator(const Widget& rhs)
			{
				... 
				return *this; //返回左侧对象
			}
			...
		};
这个协议不仅适用于以上的标准赋值形式，也适用于所有赋值相关运算，例如：

		class Widget{
		public:
			...
			Widget& operator+=(const Widget& rhs)
			{
				...
				return *this;
			}
		}