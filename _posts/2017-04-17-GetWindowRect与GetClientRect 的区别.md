---
title: GetWindowRect与GetClientRect 的区别
date: 2016-04-25 15:00:02
tags: CSDN迁移
---
   **GetWindowRect  
 函数功能：该函数返回指定窗口的边框矩形的尺寸。该尺寸以相对于屏幕坐标左上角的屏幕坐标给出。  
 函数原型：BOOL GetWindowRect（HWND hWnd，LPRECTlpRect）；  
 在Visual Studio 2005中，函数原型为void GetWindowRect(LPRECT lpRect) const;  
 是属于CWnd类的函数.  
 参数：  
 hWnd:窗口句柄。  
 lpRect：指向一个RECT结构的指针，该结构接收窗口的左上角和右下角的屏幕坐标。  
 返回值：如果函数成功，返回值为非零：如果函数失败，返回值为零。若想获得更多错误信息，请调用GetLastError函数。  
 速查：Windows NT：3.1以上版本：Windows：95以上版本；Windows CE：1.0以上版本；头文件：Winuser.h；库文件：User32.lib。  
   
 先调用GetWindowRect后再调用ScreenToClient,这个时候得到的rect和直接使用GetClientRect得到的值是相等的。**

 **有时候需要获得窗口矩形的大小和客户区矩形的大小二者的值，故需要分别调用GetWindowRect和GetClientRect。**

 **如果只需要获得客户区矩形的大小，调用GetClientRect就行了。**

 **GetWindowRect和GetClientRect函数的说明如下：**

 **CWnd::GetClientRect   
 void GetClientRect( LPRECT lpRect ) const;  
 Parameters:  
 lpRect  
 Points to a RECT structure or a CRect object to receive the client coordinates. The left and top members will be 0. The right and bottom members will contain the width and height of the window.  
 Remarks:  
 Copies the client coordinates of the CWnd client area into the structure pointed to by lpRect. The client coordinates specify the upper-left and lower-right corners of the client area. Since client coordinates are relative to the upper-left corners of the CWnd client area, the coordinates of the upper-left corner are (0,0).**

 **CWnd::GetWindowRect  
 void GetWindowRect( LPRECT lpRect ) const;  
 Parameters:  
 lpRect  
 Points to a CRect object or a RECT structure that will receive the screen coordinates of the upper-left and lower-right corners.  
 Remarks:  
 Copies the dimensions of the bounding rectangle of the CWnd object to the structure pointed to by lpRect. The dimensions are given in screen coordinates relative to the upper-left corner of the display screen. The dimensions of the caption, border, and scroll bars, if present, are included.**

 **GetWindowRect() 得到的是在屏幕坐标系下的RECT；（即以屏幕左上角为原点）   
 GetClientRect() 得到的是在客户区坐标系下的RECT； （即以所在窗口左上角为原点）**

 **GetWindowRect()取的是整个窗口的矩形；   
 GetClientRect()取的仅是客户区的矩形，也就是说不包括标题栏，外框等；**

 **第一个函数获得的是窗口在屏幕上的位置，得到的结果可能是这样CRect(10,10,240,240);   
 第二个函数和它不同，它只获得了客户区的大小，因此得到的结果总是这样CRect(0,0,width,height);**

 **ScreenToClient() 就是把屏幕坐标系下的RECT坐标转换为客户区坐标系下的RECT坐标。**

 **The GetClientRect function retrieves the coordinates of a window's client area. The client coordinates specify the upper-left and lower-right corners of the client area. Because client coordinates are relative to the upper-left corner of a window's client area, the coordinates of the upper-left corner are (0,0).**

 **GetClientRect得到的是客户区的大小，也就是说这样得到的左上角永远是（0，0）**

 **The GetWindowRect function retrieves the dimensions of the bounding rectangle of the specified window. The dimensions are given in screen coordinates that are relative to the upper-left corner of the screen.**

 **GetWindowRect 是窗口相对于整个屏幕的坐标，屏幕左上点为0，0**

 **相互转化用ScreenToClient 或者 ClientToScreen**

 **ClientToScreen  
 The ClientToScreen function converts the client coordinates of a specified point to screen coordinates.   
 BOOL ClientToScreen(  
 HWND hWnd, // window handle for source coordinates  
 LPPOINT lpPoint // pointer to structure containing screen coordinates  
 );  
 Parameters  
 hWnd   
 Handle to the window whose client area is used for the conversion.   
 lpPoint   
 Pointer to a POINT structure that contains the client coordinates to be converted. The new screen coordinates are copied into this structure if the function succeeds.   
 Return Values  
 If the function succeeds, the return value is nonzero.  
 If the function fails, the return value is zero.**

 **虽然存在调用GetWindowRect后再调用ScreenToClient==GetClientRect，但ScreenToClient（）和ClientToScreen()两者都是属于WINDOWS API函数，可能是存在一定的冗余设计，但意义不同。  
 不过在.Net Framework下对WINDOWS API函数进行了重新整理和优化，在获取控件或窗口的屏幕坐标和客户区坐标时更方便的多，只需要得到与控件或窗口相对应屏幕坐标和客户区坐标属性值就可以了。**

 **ScreenToClient  
 The ScreenToClient function converts the screen coordinates of a specified point on the screen to client coordinates.   
 BOOL ScreenToClient(  
 HWND hWnd, // window handle for source coordinates  
 LPPOINT lpPoint // address of structure containing coordinates  
 );  
 Parameters：  
 hWnd   
 Handle to the window whose client area will be used for the conversion.   
 lpPoint   
 Pointer to a POINT structure that contains the screen coordinates to be converted.   
 Return Values：  
 If the function succeeds, the return value is nonzero.  
 If the function fails, the return value is zero.**

 ** **

 **本文来自CSDN博客，转载请标明出处：[http://blog.csdn.net/dadalan/archive/2010/02/22/5316599.aspx](http://blog.csdn.net/dadalan/archive/2010/02/22/5316599.aspx)**

   
 [](http://blog.csdn.net/ke_yang/article/details/5417552#)[](http://blog.csdn.net/ke_yang/article/details/5417552#)[](http://blog.csdn.net/ke_yang/article/details/5417552#)[](http://blog.csdn.net/ke_yang/article/details/5417552#)[](http://blog.csdn.net/ke_yang/article/details/5417552#)[](http://blog.csdn.net/ke_yang/article/details/5417552#)  
 **顶**  
   
 