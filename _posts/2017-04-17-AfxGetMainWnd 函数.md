---
title: AfxGetMainWnd 函数
date: 2016-08-18 15:54:19
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/52241405   
   **AfxGetMainWnd( )：**

 **  
**

 **使用AfxGetMainWnd函数获取MFC程序中的主框架类指针是一个常用作法。**

 **  
**

 **就是获得应用程序主窗口的指针，AfxGetMainWnd()-> m_hWnd是主窗口的句柄。**

 **在MFC中调用api函数的时候，要在前面加上:: 不然的话会出现错误。**

 **  
**

 **  
**

 **关于好多MFC的函数都有Afx开头：**

 **  
**

 **Application Frameworks 应用程序框架**

 **  
 这是MS最早想统一各种平台C++开发的一个类库，**

 **  
 但是历时一年后失败了，**

 **  
 开发人员将它简化，得到了MFC。**

 **  
 A和F的意义很好理解，而X则是ks的一种读音的近似，这在英语中很常见。  
  
  
 AFX开头的函数是最早的MFC开发小组用的函数头，后来就沿用了下来。  
**

   
 