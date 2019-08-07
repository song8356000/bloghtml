---
title: 编写dll 关于declspec(dllexport)和declspec(dllimport)
date: 2018-06-05 16:58:11
tags: CSDN迁移
---
   使用api 要先懂得怎么使用dll文件；  
1.新建一个常规dll   
//dll.h  
#ifndef DLL_H  
#define DLL_H  
  
  
#ifdef DLL_EXPORTS  
#define DLLEXPORT __declspec(dllexport)  
#else  
#define DLLEXPORT __declspec(dllimport)  
#endif  
  
  
DLLEXPORT int add(int ,int);  
  
  
#endif  
  
  
//中间一段的意思是： 如果在工程里添加预定义宏DLL_EXPORTS 那DLLEXPORT 就指代__declspec(dllexport) 用于dll的导出（函数 变量 类等）  
导出变量用 __declspec(dllexport) int a；  
导出函数用 __declspec(dllexport) void foo（）；  
导出类用 class __declspec(dllexport) a{};  
如果应用程序需要调用dll中的函数,则需要用__declspec(dllimport)修饰，因此当工程中不包含DLL_EXPORTS预定义时，DLLEXPORT 就指代__declspec(dllimport) 这样生成dll程序和调用dll的程序可以使用同一个头文件  
（ps：工程中会自动添加一个DLL_EXPORTS预定义，在属性—c\c++ —preprocesspr里面）  
//dll.cpp  
#include "dll.h"  
  
  
int add(int x,int y){  
return x+y;  
}  
这样在工程的debug目录下就生成了dll和lib文件  
  
  
2.调用dll文件  
新建一个控制台程序calldll  
添加c++文件  
//calldll.cpp  
#include "dll.h"  
#include <iostream>  
int main(){  
int z;  
int x=1, y=2;  
z=add(x,y);  
std::cout<<z<<std::endl;  
}  
工程属性里设置  
1）c\c++ - general - additional include directories里面添加头文件目录  
2）linker -additional library directories 里添加lib文件的目录  
3）linker -input -additonal dependencies 里添加lib的文件名  
 编译通过后用行，会提示丢失dll.dll文件   
解决：将dll.dll放在和exe同一目录下或放在c:\windows\system32目录下（Win64位操作系统且dll.dll不是64位而是32位的话放在c:\windows\syswow64目录下）

原文地址：http://blog.sina.com.cn/s/blog_a7e72e940101izkh.html

   
 