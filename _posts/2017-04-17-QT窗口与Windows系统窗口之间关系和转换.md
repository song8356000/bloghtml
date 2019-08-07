---
title: QT窗口与Windows系统窗口之间关系和转换
date: 2019-05-07 10:41:02
tags: CSDN迁移
---
   如果你是通过qt开发windows应用程序，是否有下面这个想法呢？

 怎么样才能将windows下对窗口的操作应用在qt窗口上呢？

 下面给出方案：

 1，众所周知，windows窗口有一个hwnd，即句柄，可以通过句柄来指代窗口。qt对话框的winId()方法可以获取句柄。

 2，windows api中的

 
```
 HWND WINAPI FindWindow(
  _In_opt_  LPCTSTR lpClassName,
  _In_opt_  LPCTSTR lpWindowName
);
```
 

 
```
 该函数通过创建窗口时的类名和窗口名查找窗口并返回该窗口的句柄，函数不会搜索子窗口。该函数区分大小写。
参数
lpClassName [in, optional]
Type: LPCTSTR
类名和窗口名是在先前调用RegisterClass or RegisterClassEx时创建的
如果lpClassName为NULL，他会寻找所有和lpWindowName参数匹配的窗口
lpWindowName [in, optional]
Type: LPCTSTR
窗口的名字也是窗口的title. 如果该参数为NULL,所有窗口名字都是匹配的
```
 函数可以通过窗口的className和windowName来查找窗口

 3，可以通过函数

 

 
```
 void setWindowTitle(const QString &)
```
 设置qt窗口的windowTitle属性，这样就可以通过windows api 中的 findwindow 函数查找到窗口的hwnd了。然后就可以利用hwnd对采用windows原生方式对窗口进行操作了，比如sendmessage/postmessage  
 --------------------- 

 感谢原作者的分享！  
 原文：https://blog.csdn.net/jigetage/article/details/79740101   
 

   
 