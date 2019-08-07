---
title: C++ const与static
date: 2019-03-13 11:52:51
tags: CSDN迁移
---
   C++ const 允许指定一个语义约束，编译器会强制实施这个约束，允许程序员告诉编译器某值是保持不变的。如果在编程中确实有某个值保持不变，就应该明确使用const，这样可以获得编译器的帮助。

 
# **一.const**

 **1.const 修饰成员变量** 

 
```
 #include<iostream>
using namespace std;
int main(){
    int a1=3;   ///non-const data
    const int a2=a1;    ///const data

    int * a3 = &a1;   ///non-const data,non-const pointer
    const int * a4 = &a1;   ///const data,non-const pointer
    int * const a5 = &a1;   ///non-const data,const pointer
    int const * const a6 = &a1;   ///const data,const pointer
    const int * const a7 = &a1;   ///const data,const pointer

    return 0;
}
```
 const修饰指针变量时：

 (1)只有一个const，如果const位于*左侧，表示指针所指数据是常量，不能通过解引用修改该数据；指针本身是变量，可以指向其他的内存单元。

 (2)只有一个const，如果const位于*右侧，表示指针本身是常量，不能指向其他内存地址；指针所指的数据可以通过解引用修改。

 (3)两个const，*左右各一个，表示指针和指针所指数据都不能修改。

 **2.const修饰函数参数**

 传递过来的参数在函数内不可以改变，与上面修饰变量时的性质一样。

 
```
 void testModifyConst(const int _x) 
{
     _x=5;　　　///编译出错
}
```
 **3.const修饰成员函数**

 (1)const修饰的成员函数不能修改任何的成员变量(**mutable修饰的变量除外**)

 (2)const成员函数不能调用非onst成员函数，因为非const成员函数可以会修改成员变量

 
```
 #include <iostream>
using namespace std;
class Point{
    public :
    Point(int _x):x(_x){}

    void testConstFunction(int _x) const{

        ///错误，在const成员函数中，不能修改任何类成员变量
        x=_x;

        ///错误，const成员函数不能调用非onst成员函数，因为非const成员函数可以会修改成员变量
        modify_x(_x);
    }

    void modify_x(int _x){
        x=_x;
    }

    int x;
};
```
 ****4.const修饰函数返回值****

 (1)指针传递

 如果返回const data,non-const pointer，返回值也必须赋给const data,non-const pointer。因为指针指向的数据是常量不能修改。

 
```
 const int * mallocA(){  ///const data,non-const pointer
    int *a=new int(2);
    return a;
}

int main()
{
    const int *a = mallocA();
    ///int *b = mallocA();  ///编译错误
    return 0;
}
```
 (2)值传递

 如果函数返回值采用“值传递方式”，由于函数会把返回值复制到外部临时的存储单元中，加const 修饰没有任何价值。所以，**对于值传递来说，加const没有太多意义。**

 所以：

 不要把函数int GetInt(void) 写成const int GetInt(void)。  
 不要把函数A GetA(void) 写成const A GetA(void)，其中A 为用户自定义的数据类型。

 **在编程中要尽可能多的使用const，这样可以获得编译器的帮助，以便写出健壮性的代码。**

 

 
# **二. static**

 

 
### static

 
  1. static局部变量 将一个变量声明为函数的局部变量，那么这个局部变量在函数执行完成之后不会被释放，而是继续保留在内存中 
  3. static 全局变量 表示一个变量在当前文件的全局内可访问 
  5. static 函数 表示一个函数只能在当前文件中被访问 
  7. static 类成员变量 表示这个成员为全类所共有 
  9. static 类成员函数 表示这个函数为全类所共有，而且只能访问静态成员变量 
### static关键字的作用：

 （1）函数体内static变量的作用范围为该函数体，该变量的内存只被分配一次，因此其值在下次调用时仍维持上次的值；   
 （2）在模块内的static全局变量和函数可以被模块内的函数访问，但不能被模块外其它函数访问；   
 （3）在类中的static成员变量属于整个类所拥有，对类的所有对象只有一份拷贝；   
 （4）在类中的static成员函数属于整个类所拥有，这个函数不接收this指针，因而只能访问类的static成员变量。

   
 