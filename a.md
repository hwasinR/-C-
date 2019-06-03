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