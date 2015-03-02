#Swift中的Class和Struct

我感觉它们的根本区别是一个引用类型和一个值类型，在《Swift By Tourist》中是这么描述的：

>- With constant classes you can mutate the properties of the class,but cannot reassign the constant to a different or new class instance.
- With constant structs you cannot mutate their properties or re-assign to a new value.

翻译过来大概就是说：let 声明的 class 类型不能再进行赋值别的本类实例操作了,但是可以进行值的修改。let 声明的 struct 既不能进行实例赋值也不能进行值的修改。

##为什么要写Override

In Siwft ,**if the method you're overriding is an exsiting method on the class,then you must add the override keyword**.This makes it clear to whoever's reding the code that behavior from the superclass may be overridden.

The override keyword also hepls the compiler make decisions about the method you're implementing and provide extra checking. For example, if you misspell the overridden method name, then the complier will throw an error because the method doesn't exit in the subclass.Likewise, if you had no idea there was such thing as viewDidLoad and just happend to pick that method name yourself, the complier would throw an error.