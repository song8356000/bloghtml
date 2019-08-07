---
title: ReleaseDC和DeleteDC的区别
date: 2016-04-15 19:24:38
tags: CSDN迁移
---
   **簡單的說，GetDC和ReleaseDC的調用配對，CreateDC和DeleteDC的調用配對。  
 GetDC是從窗口獲取現有的DC，而CreateDC是創建DC，所以ReleaseDC和DeleteDC的作用一個是釋放，一個是銷毀。**

 **ReleaseDC和DeleteDC的区别 (转)   
 在编SDK小游戏时发现的图片在背景上移动时，可能出现闪烁，这时双缓冲就是基本方法之一。  
 但发现一个480 * 580的小窗口中，都是移动了185次后，图片不动了，百思不得解，试想为内存泄漏  
 原来是ReleaseDC和DeleteDC在做怪，对于create的dc应该予以deletedc，而对于getdc的应予以releasedc  
 附上图片移动源码：  
 #include "windows.h"  
 LRESULT CALLBACK WndProc (HWND, UINT, WPARAM, LPARAM);  
 void showpic();  
 HWND hwnd;  
 int xp,yp;  
 int WINAPI WinMain( HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine,int nCmdShow)  
 {  
 WNDCLASS wndcls;  
 wndcls.cbClsExtra=0;  
 wndcls.cbWndExtra=0;  
 wndcls.hbrBackground=(HBRUSH)GetStockObject(BLACK_BRUSH);  
 wndcls.hCursor=(HCURSOR)LoadCursor(NULL,IDC_ARROW);  
 wndcls.hIcon=(HICON)LoadIcon(NULL,IDI_WINLOGO);  
 wndcls.hInstance=hInstance;  
 wndcls.lpfnWndProc=WndProc;  
 wndcls.lpszClassName="bao";  
 wndcls.lpszMenuName=NULL;  
 wndcls.style=CS_HREDRAW | CS_VREDRAW;  
 RegisterClass(&wndcls);  
 hwnd=CreateWindow("bao","xxx",WS_OVERLAPPEDWINDOW | WS_VISIBLE,CW_USEDEFAULT,CW_USEDEFAULT,480,580, NULL,NULL,hInstance,NULL);  
 MSG msg;  
 while (GetMessage(&msg,NULL,0,0))  
 {  
 TranslateMessage(&msg);  
 DispatchMessage(&msg);  
 }  
 return msg.wParam;  
 }  
 LRESULT CALLBACK WndProc(HWND hWnd, UINT uMsg, WPARAM wparam, LPARAM lparam)   
 {  
 HDC hdc=GetDC(hWnd);  
 switch(uMsg)  
 {  
 case WM_KEYDOWN:  
 if (wparam==VK_LEFT)  
 {  
 xp-=100;  
 if (xp"0) xp=0;  
 }  
 if (wparam==VK_UP)  
 {  
 yp-=100;  
 if (yp"0) yp=0;  
 }  
 if (wparam==VK_RIGHT)  
 {  
 xp+=100;  
 if (xp"440) xp=440;  
 }  
 if (wparam==VK_DOWN)  
 {  
 yp+=100;  
 if (yp"500) yp=500;  
 }  
 showpic();  
 break;  
 case WM_DESTROY:  
 PostQuitMessage(0);  
 }  
 return DefWindowProc(hWnd,uMsg,wparam,lparam);  
 }  
 void showpic()  
 {  
 HDC hdc=GetDC(hwnd);  
 HDC hmemdc=CreateCompatibleDC(hdc);  
 HBITMAP hbc=CreateCompatibleBitmap(hdc,480,580);  
 SelectObject(hmemdc,hbc);  
 BitBlt(hdc,0,0,480,580,hmemdc,0,0,SRCCOPY);  
 DeleteObject(hbc);  
 DeleteDC(hmemdc);  
 //换成ReleaseDC(hwnd,hmemdc);将出现内存泄漏，将导致图片停止移动  
 ReleaseDC(hwnd,hdc);  
 }  
**

   
 [](http://blog.csdn.net/angxiao/article/details/7488320#)[](http://blog.csdn.net/angxiao/article/details/7488320#)[](http://blog.csdn.net/angxiao/article/details/7488320#)[](http://blog.csdn.net/angxiao/article/details/7488320#)[  
](http://blog.csdn.net/angxiao/article/details/7488320#)  
   
 