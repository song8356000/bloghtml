---
title: Run-Time Check Failure #0，The value of ESP was not properly saved 错误解决
date: 2018-06-06 15:10:15
tags: CSDN迁移
---
   调用DLL函数，出现错误

Run-Time Check Failure #0 - The value of ESP was not properly saved across a function call. This is usually a result of calling a function declared with one calling convention with a function pointer declared with a different calling convention.

错误原因：

你定义函数指针原型时出错。

其实你定义的没有错，但是编译器不认识而已，因为你调用的dll函数是一个远函数，而且是一个C函数，你得告诉编译器它是个c函数才行。那么你就可以在定义该函数的时候加上一句话，

FAR PASCAL 或者 __stdcall 这个就OK了。

具体做法：

比如说你要定义一个 返回类型为空，参数为空的函数指针：

typedef void （*LPFUN）（void）;

这样确实跟我们dll里的函数匹配了，上面也说了，我们应该添上几个字，告诉编译器这个是一个远的C函数。

typedef void （WINAPI *LPFUN）（void）;

typedef void （__stdcall *LPFUN）（void）;

typedef void (FAR PASCAL *LPFUN) (void);

像上面这样定义就OK了，如果用的是VC++，那么直接用第一种定义就ok了。

注意，上面是使用 MFC （DLL）的做法。

如果是WIN32 DLL，得相应的去掉WINAPI ，__stdcall ，FAR PASCAL 这几个参数。因为WIN32 DLL 默认的入栈方式为 __cedcall方式，不是__stdcall方式。

具体的组合方式太多了，反正知道错误的原因是声明相应的函数未匹配就行了。







调用DLL里的函数 或 类成员函数 碰到此错误：

Run-Time Check Failure #0 - The value of ESP was not properly saved across a function call. This is usually a result of calling a function declared with one calling convention with a function pointer declared with a different calling convention.



函数定义的调用规则,和实际的调用规则不同。如 编译器默认的是__cdecl，而__stdcall 类型的函数却用了 __cdecl 的调用规则，由于编译时不会报错，结果出现了运行时异常。

所以把在函数定义中进行设置调用规则即可解决此问题。

如：  typedef void (__stdcall Foo)(int a);







很久没写代码，一天几行的代码:

typedef int ( *PFUN)(HWND hWnd, LPCTSTR lpText, LPCTSTR lpCaption, UINT uType);  
void CTestProcessMonitorDlg::OnBnClickedButton1()  
{  
 // TODO: Add your control notification handler code here  
  
 //MessageBox(TEXT("Hello"), TEXT("Test"));  
  
 //typedef void (*pfv) ();  
  
 HMODULE hmod = ::LoadLibraryExW(TEXT("user32.dll"), NULL, 0);  
 if (hmod != NULL)  
 {  
 PFUN pFun= (PFUN)GetProcAddress(hmod, "MessageBoxW");  
 if (pFun != NULL)  
 {  
 pFun(m_hWnd, TEXT("Hello"), TEXT("Test"), MB_YESNO);  
 }

:FreeLibrary(hmod);  
 }  
  
   
   
}  
  


突然出现以下的错误：  


Run-Time Check Failure #0 - The value of ESP was not properly saved across a function call. This is usually a result of calling a function declared with one calling convention with a function pointer declared with a different calling convention.

![](http://hi.csdn.net/attachment/201201/25/0_1327486107LXb7.gif)  




代码改为如下， 则问题没有了

typedef int (WINAPI *PFUN)(HWND hWnd, LPCTSTR lpText, LPCTSTR lpCaption, UINT uType);  
void CTestProcessMonitorDlg::OnBnClickedButton1()  
{  
 // TODO: Add your control notification handler code here  
  
 //MessageBox(TEXT("Hello"), TEXT("Test"));  
  
 //typedef void (*pfv) ();  
  
 HMODULE hmod = ::LoadLibraryExW(TEXT("user32.dll"), NULL, 0);  
 if (hmod != NULL)  
 {  
 PFUN pFun= (PFUN)GetProcAddress(hmod, "MessageBoxW");  
 if (pFun != NULL)  
 {  
 pFun(m_hWnd, TEXT("Hello"), TEXT("Test"), MB_YESNO);  
 }

::FreeLibrary(hmod);  
 }  
   
}

   
 