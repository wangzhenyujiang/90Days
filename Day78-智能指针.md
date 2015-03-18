#一个简单的智能指针类：

在简历上写了 iOS 的 ARC 和 C++ 中的智能指针差不多，被面试官问到这一点了，因为好长时间没有看没有回答出来感觉好冤。今天重新看了一下，感觉好还，在此记一下：

这个方法是通过引用计数来实现的，《C++ Primer》上面的第 15.8.1 节中还记录了另外一个方法。

首先，我们要定义自己的一个智能指针的类，用户持有指向这个类的指针，而这个指针类才真正的指向数据。

看下面一个 HasPrt 类：

```C++
#ifndef U_Ptr_HasPrt_h
#define U_Ptr_HasPrt_h
#include"U_Prt.h"

class HasPrt{
public:
    //HasPrt(int *p,int i):prt(p),val(i){}
    HasPrt(int *p,int i):prt(new U_Prt(p)),val(i){}
    
    HasPrt(const HasPrt &oring):prt(oring.prt),val(oring.val){
        ++prt->use;
    }
    HasPrt& operator = {const HasPrt&};
    ~HasPrt(){
        if (--prt->use == 0) {
            delete prt;
        }
    }
    
    int *get_prt const {return prt;}
    int get_val const {return val;}
    
    void set_prt(int *prt){self.prt = prt;}
    void set_val(int val){self.val = val;}
    
    int get_prt_val() const { return *prt;}
    void set_prt_val(int i) const { *prt = i;}
    
private:
    //int *prt;
    U_Prt *prt;
    int val;
}

HasPrt& HasPrt::operator = (const HasPrt &rhs)
{
    ++rhs.prt -> use;
    if (--prt -> use == 0) {
        delete prt;
    }
    
    prt = rhs.prt;
    val = rhs.val;
    
    return *this;
}

#endif

```

再看 U_Prt 类：

```C++
#ifndef U_Ptr_U_Prt_h
#define U_Ptr_U_Prt_h

#include"HasPrt.h"

class U_Prt{
    
    friend class HasPrt;
    
private:
    int *ip;
    size_t use;
    
    U_Prt(int *p):ip(p),use(1){}
    ~U_Prt(){ delete ip;}
    
}
#endif

```