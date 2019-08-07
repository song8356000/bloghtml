---
title: c 调用GetConsoleWindow () 几个解决方法
date: 2016-09-06 16:44:00
tags: CSDN迁移
---
   # [c 调用GetConsoleWindow () 几个解决方法](http://www.cnblogs.com/yhyjy/archive/2012/02/28/2370977.html)

   
 要么在文件头写上：#define _WIN32_WINNT 0x0500

 要么在文件头写上：wincon.h

 要么在文件头写上：extern "C" WINBASEAPI HWND WINAPI GetConsoleWindow ();

   
   
   
 