---
title: HINSTANCE HANDLE  HWND 的区别及一般方法
date: 2016-04-05 16:53:25
tags: CSDN迁移
---
   **HINSTANCE是应用程序实例句柄，  
  
  
 HWND是窗口对象句柄，  
  
  
 HANDLE是任意对象的句柄，  
  
  
 CWnd是MFC中的窗口类。  
  
  
  
  
  
  
 MSDN里面对于HINSTANCE的解释是"handle to an instance" 就是说是一个instance的句柄  
  
  
 。而对instance的解释是"An object for which memory is allocated or which is   
  
  
 persistent." 占有内存的一个对象。  
  
  
 对于HWND的解释是“Handle to a window.”而对window的解释是"In a graphical   
  
  
 Windows-based application, a window is a rectangular area of the screen where   
  
  
 the application displays output and receives input from the user. Therefore,   
  
  
 one of the first tasks of a graphical Windows-based application is to create a   
  
  
 window. " 就是说是屏幕上的一块区域。  
  
  
 CWnd是MFC的一个类了，它有窗体，几乎所有有图形显示的类都是从它派生的，它自己是从  
  
  
 CCmdTarget类派生的，所以它可以接受消息。CCmdTarget类的爸爸可就是CObject了。  
  
  
 msdn对于Handle的解释是"Handle to an object." ,简直是废话。自己怎么解释自己呢。  
  
  
 可是好像也只能如此了。我感觉句柄就可以理解为控制对象的一个…………东西吧。  
  
  
 ------------------------------------------------------------------------------  
  
  
 ID--HANDLE--HWND三者之间的互相转换  
 ID--HANDLE--HWND三者之间的互相转换  
 id->句柄-----------hWnd = ::GetDlgItem(hParentWnd,id);  
 id->指针-----------CWnd::GetDlgItem();  
 句柄->id-----------id = GetWindowLong(hWnd,GWL_ID);  
 句柄->指针--------CWnd *pWnd=CWnd::FromHandle(hWnd);  
 指针->ID----------id = GetWindowLong(pWnd->GetSafeHwnd,GWL_ID);  
 GetDlgCtrlID();  
 指针->句柄--------hWnd=cWnd.GetSafeHandle() or mywnd->m_hWnd;  
  
  
 -------------------------------------------------------------------------------  
  
  
 应用程序的一些HANDLE  
  
  
 // 得到窗口句柄  
 HWDN parenthwnd = ::FindWindowEx(NULL, parenthwnd, "#32770", NULL);  
 // 得到此窗口的主线程ID  
 DWORD dwThreadId = ::GetWindowThreadProcessId(parenthwnd, 0);  
 // 得到当前进程的句柄  
 HANDLE hApp = GetModuleHandle(NULL);**  
   
 