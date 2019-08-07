---
title: C++中 explicit的用法
date: 2017-10-27 16:03:55
tags: CSDN迁移
---
   explicit 是避免构造函数的参数自动转换为类对象的标识符

 

 [cpp] [view plain](http://blog.csdn.net/acdnjjjdjkdckjj/article/details/5644573#) [copy](http://blog.csdn.net/acdnjjjdjkdckjj/article/details/5644573#)   
   
   
 
  1. #include <iostream> 
  2. using namespace std; 
  3. 
  4. class A 
  5. { 
  6. public: 
  7. 
  8. explicit A(int a) 
  9. { 
  10. cout<<"创建类成功了!"<<endl; 
  11. 
  12. 
  13. } 
  14. 
  15. 
  16. 
  17. }; 
  18. int main() 
  19. { 
  20. 
  21. A a=10; 
  22. return 0; 
  23. }   
 

 

 上面的代码编译不成功，原因是当显式地定义了一个带一个参数的构造函数（ 带explicit），必须要显示地调用构造函数，

 A a（10）；

 如果不加 explicit的话

 A a=10；

 实际的转换过程如下：  
 相当于直接调用A（10）；

 （1）

 explicit

 此关键字只能对用户自己定义的对象起作用，不对默认构造函数起作用  
 此关键字只能够修饰构造函数。而且构造函数的参数只能有一个。。

 （2）何时用explicit

 当我们不希望自动类型转换的时候用，其实标准库好多构造函数都是explicit的

 比如说vector <int> ivec(10); //这种定义看起来一目了然

 不能写成vector <int> ivec=10；//此种定义让程序员感到疑惑

 （3）何时不用explicit

 当我们需要隐式转换的时候

 比如说String类的一个构造函数

 String(const char*);

 

 定义成这样的好处,在需要隐式转化的时候编译器会自动地帮我们转换，标准库里面的String就是一个好的证明。

 具体来说：

 

 我们可以这样String str="helloworld";//直接调用构造函数

 

 String str="hello"+str+"world";

 

 调用重载的+操作符号，此过程相当于：  
 String temp("hello"); //调用构造函数

 String str=temp+str;

 String t("world");//调用构造函数

 String str=str+t;

 

 

 明白隐式转换在我们自己写类的时候，尤其是些操纵内存的类的时候很有用。

 

   
 