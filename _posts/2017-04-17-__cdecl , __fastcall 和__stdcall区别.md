---
title: __cdecl , __fastcall 和__stdcall区别
date: 2018-06-05 17:32:51
tags: CSDN迁移
---
   三者都称为调用约定（ Calling convention ）

1）决定函数参数的压栈顺序

2）由调用者还是被调用者把参数弹出栈

3）编译产生函数名的方式

__stdcall 程序如果使用了__stdcall 关键字，或者设置了/Gz 编译选项，__stdcall 这种调用方式就会生效，WIN32 API都采用__stdcall调用方式。


  * 函数参数采用从右到左的压栈方式；
  * 被调函数在返回前自己清空堆栈；
  * 输出函数名前面加下划线，后面加“@”符号和参数的字节数，如[_funname@123](mailto:_funname@123)。__cdecl 程序如果使用了__cdecl关键字，或者设置了/Gd 编译选项（C/C++的缺省调用方式），__cdecl调用方式会生效。


  * 函数参数采用从右到左的压栈方式；
  * 传送参数的内存栈由调用者维护，每一个调用它的主调函数都包含清空堆栈的代码；
  * 输出函数名仅在函数名前加下划线，如：_funname；
  * 由于_cdecl调用方式的参数内存栈由调用者维护，所以变长参数的函数能（也只能）使用这种调用约定，比如 printf(const char*,...)；__fastcall 程序如果使用了__fastcall 关键字，或者设置了/Gr 编译选项，__fastcall 调用方式会生效。


  * 它通过CPU内部寄存器ECX和EDX传送前两个双字（DWORD）或更小的参数，剩下的参数仍旧自右向左压栈传送；
  * 被调用的函数在返回前清理传送参数的内存栈；
  * __fastcall调用约定在输出函数名前面加“@”符号，后面加“@”符号和参数的字节数，形如@funcname@123









cdecl、pascal、stdcall、fastcall做函数修饰符号用，告诉编译器函数参数压栈顺序，压入堆栈的内容由谁来清除，调用者还是函数自己？  
  
调用约定 压参数入栈顺序 把参数弹出栈者   
__cdecl 右->左 调用者   
__stdcall 右->左 被调用者   
__pascal 左->右 被调用者   
__fastcall 右->左 被调用者 


  1. __cdecl是C/C++和MFC程序默认使用的调用约定，也可以在函数声明时加上__cdecl关键字来手工指定。采用__cdecl约定时，函数参数按照从右到左的顺序入栈，并且由调用函数者把参数弹出栈以清理堆栈。因此，实现可变参数的函数只能使用该调用约定。由于每一个使用__cdecl约定的函数都要包含清理堆栈的代码，所以产生的可执行文件大小会比较大。__cdecl可以写成_cdecl。
  2. __stdcall调用约定用于调用Win32 API函数。采用__stdcal约定时，函数参数按照从右到左的顺序入栈，被调用的函数在返回前清理传送参数的栈，函数参数个数固定。由于函数体本身知道传进来的参数个数，因此被调用的函数可以在返回前用一条ret n指令直接清理传递参数的堆栈。__stdcall可以写成_stdcall。
  3. __fastcall约定用于对性能要求非常高的场合。__fastcall约定将函数的从左边开始的两个大小不大于4个字节（DWORD）的参数分别放在ECX和EDX寄存器，其余的参数仍旧自右向左压栈传送，被调用的函数在返回前清理传送参数的堆栈。__fastcall可以写成_fastcall。
  4. pascal 调用协议仅用于向后兼容，即向旧的版本兼容。以上几个关键词可在“windef.h”头文件中找到：

#define CALLBACK __stdcall   
#define WINAPI __stdcall   
#define WINAPIV __cdecl   
#define APIENTRY WINAPI   
#define APIPRIVATE __stdcall   
#define PASCAL __stdcall   
#define cdecl _cdecl   
#ifndef CDECL  
#define CDECL _cdecl   
#endif   
  
特别说明  
1. 在默认情况下，采用__cdecl方式,因此可以省略.  
2. WINAPI一般用于修饰动态链接库中导出函数  
3. CALLBACK仅用于修饰回调函数 便于更好理解, 看下面例子（函数调用的过程以汇编代码表示）：   
void cdecl fun1(int x,int y);   
void stdcall fun2(int x,int y);   
void pascal fun3(int x,int y);   
****************************************   
void cdecl fun1(int x,int y);   
fun1(x,y);   
调用 fun1 的汇编代码   
push y   
push x   
call fun1   
add sp,sizeof(x)+sizeof(y) ；跳过参数区（x，y）   
fun1 的汇编代码：   
fun1 proc   
push bp   
mov bp,sp   
……   
…   
pop bp   
ret ；返回，但不跳过参数区   
fun1 endp   
****************************************   
void stdcall fun2(int x,int y);   
fun2(x,y);   
调用 fun2 的汇编代码   
push y   
push x   
call fun2   
fun2 的汇编代码：   
fun2 proc   
push bp   
mov bp,sp   
……   
…   
pop bp   
ret sizeof(x)+sizeof(y) ；返回并跳过参数区（x，y）   
fun2 endp   
*****************************************   
void pascal fun3(int x,int y);   
fun3(x,y);   
调用 fun3 的汇编代码   
push x   
push y   
call fun3   
fun3 的汇编代码：   
fun3 proc   
push bp   
mov bp,sp   
……   
…   
pop bp   
ret sizeof(x)+sizeof(y) ；返回并跳过参数区（x，y）   
fun3 endp   
   
 