#C++智能指针一

##问题的由来

####一个带指针成员的简单类

	class HasPtr{
	public:
		HasPtr(int *p,int i):ptr(p),val(i){}
		
		int *get_ptr() const { return ptr; }
		int get_int() const { return val; }
		
		void set_ptr(int *p) { ptr = p; }
		void set_int(int i) { val = i; }
		
		int get_ptr_val() const { return *ptr };
		void set_ptr_val(int i) { *ptr = i }; 
	private:
		int *ptr;
		int val;
	}

####默认复制/赋值与指针成员

因为HasPtr类没有定义复制构造函数，所以复制一个HasPtr对象将复制两个成员：

	int obj=0;
	HasPtr ptr1( &obj , 42 );
	HasPtr ptr2( ptr1 );
	
复制之后，ptr1和ptr2中的指针成员指向**同一对象**并且两个对象中得int值相同。

####指针共享同一对象

	ptr1.set_ptr_val(42);
	ptr2.get_ptr_val();   //return 42
	
因为ptr1和ptr2中的指针成员指向同一个对象，所以任意一个修改对象的值将会影响另一个。

####可能出现悬垂指针

	int *ip = new int(42);
	HasPtr ptr(ip,10);
	delete ip;
	ptr.set_ptr_val(15);  //error
	
这里的问题是ip和ptr中的指针指向同一对象。删除了该对象时，ptr中的指针不再指向有效对象。然而，没有办法得知对象已经不存在了。

	 