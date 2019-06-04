# 2. 头文件与类的声明
## include 两种方式的区别
<img src=".\headerDiff.jpg" width = "1150" height = "380" alt="cpp & h file" align=center>  

\# include"" 引用的是程序目录的相对路径中的头文件。   
\# include&lt; \> 编译器的类库路径里面的头文件。它会先在你项目的当前目录查找是否有对应头文件，如果没有，它还会在对应的引用目录里面查找对应的头文件。

## 头文件防卫式声明
```
#ifndef _COMPLEX_
#define _COMPLEX_
#endif
```
# 3.构造函数  
## 构造函数
### initialization list
```
complex(double r = 0,double i = 0):re(r),im(i){

}
```
### private ctor (Singleton mode)
```
class A{
public:
    static A& getInstance();
    setup(){...}
private:
    A();
    A(const A& rhs);
    ...
};

A& A:getInstance()
{
    static A a;
    return a;
}
A::getInstance().setup(); // 静态方法通过类名调用
```

# 4. 参数传递与返回值
## const member function 常量成员函数：不改变对象数据

## pass by reference or value
### 参数传递：最好传引用
### 返回值传递：尽量传引用
如果有 temp object，必须传值

## 相同 class 的各个 object 互为友元


# 5. 操作符重载和临时对象
```
c2 += c1;
inline complex& complex::operator += (this,const complex& r)
{
    return _doapl(this,r);
}
```
this 指针指向操作符的左边，即 c2。谁调用 this 指向谁。

# 6. complex 类实现过程
## 为什么operator << 必须是非成员函数
成员函数包含一个 this 指针，this 指针指向操作符的左边，是调用该操作符的对象。如果是成员函数，左边的量必须是 c1。我们会写出以下代码：
```
c1 << cout;
```
```
ostream& operator <<(ostream & os,const complex& x)
```

## 7.BIG3:拷贝构造，拷贝赋值，析构
### 拷贝构造函数（构造函数第一个参数为自身类类型）
针对对象的初始化
```
inline String::String(const string& cstr = 0)
{
    if(cstr)
    {
        m_data = new char[strlen(cstr)+1];
        strcpy(m_data,cstr);
    }
    else
    {
        m_data = new char[1];
        *m_data = '\0';
    }
}
```
```
String S2 = S1;
String S2(S1);   // 两者都调用拷贝构造
```
针对带有指针的类,使用默认构造函数会造成浅拷贝的问题。
<img src=".\memory_leak due_to_ctor.jpg" width = "550" height = "380" alt="cpp & h file" align=center>  


### 拷贝赋值函数
针对已存在对象的赋值，步骤为：
1.先释放自己的内存
2.再申请待赋值对象大小的内存
3.最后赋值
```
inline String::operator =(const string& str)
{
    if(this == &str) // 检测自我赋值
       return *this;
    delete[] m_data;
    m_data = new char[strlen(str.m_data)+1];
    strcpy(m_data,str.m_data);
    return *this;
}
```
<img src=".\memory_leak_due_to_assignmentcopy.jpg" width = "550" height = "380" alt="cpp & h file" align=center> 

### 析构函数
```
inline String::~String()
{
    delete[] m_data;
}
```

# 8. 堆，栈与内存管理
develop: 2019.6.2  

<!--![avatar](\VC_memory_alloc_string.jpg)-->

<img src=".\VC_memory_alloc_string.jpg" width = "450" height = "860" alt="class with pointer" align=center>


Debug 模式比 release 模式多出了（32 + 4）的Header与 No man land
每一片内存大小均为16比特的倍数（VC）
Cookie 用来指示 delete操作符 释放内存
```
string *p = new String[3];
delete[] p; // 唤起 3 次 dtor
```
```
string *p = new String[3];
delete p; // 唤起 1 次 dtor
```
![VC memory delete](0989DC1E5A43488D9654D632345D735C)  
对于Complex类（成员无指针的类）,上述两者无区别。


this is a test