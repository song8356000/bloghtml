---
title: C++里大写TRUE和小写true区别
date: 2018-11-23 17:32:20
tags: CSDN迁移
---
   1.C++里大写TRUE和小写true区别   
 true是bool型的；   
 TRUE是int型的，VC里这个是ms自己定义的；

 C++规定不允许只通过返回类型不同区别两个函数   
  
 2.而”DWORD"和“HWND"分别指什么？   
 DWORD类型表示“双字”，也就是四字节大小的整型值，在windef.h 中，DWORD的定义如下：   
 typedef unsigned long DWORD;也就是说，DWORD 和unsigned long是一样的。   
 同样是windef.h文件中，对HWND的定义是这样的：   
 struct HWND__{int unused;};typedef HWND__* HWND;   
 也就是说HWND是一个指向HWND__类型的指针，而类型HWND__很明 显，就是一个类似占位符的东西。简单的说来，HWND就是一个指针，它用来定义窗口的句柄。   
  
 3.MFC中的”false“和 “FALSE"有没区别？   
 有区别。false是bool类型的值，一个字节大小。而FALSE是BOOL类型的值，BOOL就是typedef int BOOL，四个字节大小。虽然FALSE和false值都是1，但一个是四字节的，一个是一字节的。

   
 