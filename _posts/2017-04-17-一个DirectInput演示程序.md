---
title: 一个DirectInput演示程序
date: 2016-09-01 17:11:06
tags: CSDN迁移
---
   **一个DirectInput演示程序**

 ** 使用DirectInput8组件，获取键盘或鼠标的缓冲数据方式，并使用了事件通知将数据显示在屏幕上。程序首先创建DirectInput8对象、鼠标键盘设备，并为它们设置相应的方式，后进入主循环。主循环使用阻塞方式，等待鼠标或键盘事件到来（鼠标移动、按键、滚轮，键盘击键），以下为全部代码，并附有详细注释。程序是VC6控制台模式下的，注意Win32函数GetConsoleWindow()的使用，要首先声明它才可使用。**

 ** 该程序的事件触发思想很有用，这里用了主循环（线程）里阻塞等待，完全可以新创建线程里阻塞等待。其实这就是一个小型数据采集系统，只不过数据源是鼠标键盘硬件。**

 ** **

 **#include <stdio.h>**

 **#include <dinput.h>**

 ** **

 **#pragma comment(lib,"dxerr8.lib ")**

 **#pragma comment(lib,"dinput8.lib")**

 **#pragma comment(lib,"dxguid.lib")**

 ** **

 **#define SAMPLE_BUFFER_SIZE 16 **

 **extern "C" WINBASEAPI HWND WINAPI GetConsoleWindow ();**

 **HRESULT ReadBufferedData_kb();**

 **HRESULT ReadBufferedData_ms();**

 ** **

 **LPDIRECTINPUT8 g_pDI = NULL; **

 **LPDIRECTINPUTDEVICE8 g_pKeyboard = NULL;**

 **LPDIRECTINPUTDEVICE8 g_pMouse = NULL; **

 **HANDLE g_Event = NULL;**

 ** **

 ** **

 **int main()**

 **{**

 **/////////////////////////////////////////////////////////////////////////////////////////////**

 **//创建事件，为自动型（使用完自动置为无信号状态），初始化为无信号状态**

 ** g_Event = CreateEvent(NULL, FALSE, FALSE, NULL);**

 ** **

 **/////////////////////////////////////////////////////////////////////////////////////////////**

 **//创建DirectInput8对象**

 ** HRESULT hr;**

 ** if( FAILED( hr = DirectInput8Create( GetModuleHandle(NULL), DIRECTINPUT_VERSION,**

 ** IID_IDirectInput8, (VOID**)&g_pDI, NULL ) ) )**

 ** {**

 ** }**

 ** **

 **//－－1－－**

 **/////////////////////////////////////////////////////////////////////////////////////////////**

 **//创建DirectInput8设备（键盘）**

 ** if( FAILED( hr = g_pDI->CreateDevice( GUID_SysKeyboard, &g_pKeyboard, NULL ) ) )**

 ** return hr;**

 ** **

 **/////////////////////////////////////////////////////////////////////////////////////////////**

 **//为键盘设置格式**

 ** if( FAILED( hr = g_pKeyboard->SetDataFormat( &c_dfDIKeyboard ) ) )**

 ** return hr;**

 ** **

 **/////////////////////////////////////////////////////////////////////////////////////////////**

 **//为键盘设置行为**

 ** hr = g_pKeyboard->SetCooperativeLevel( GetConsoleWindow(), DISCL_BACKGROUND | DISCL_NONEXCLUSIVE );**

 ** **

 **/////////////////////////////////////////////////////////////////////////////////////////////**

 **//为键盘设置缓冲方式**

 ** DIPROPDWORD dipdw;**

 ** **

 ** dipdw.diph.dwSize = sizeof(DIPROPDWORD);**

 ** dipdw.diph.dwHeaderSize = sizeof(DIPROPHEADER);**

 ** dipdw.diph.dwObj = 0;**

 ** dipdw.diph.dwHow = DIPH_DEVICE;**

 ** dipdw.dwData = SAMPLE_BUFFER_SIZE; // Arbitary buffer size**

 ** **

 ** if( FAILED( hr = g_pKeyboard->SetProperty( DIPROP_BUFFERSIZE, &dipdw.diph ) ) )**

 ** return hr;**

 ** **

 ** **

 **/////////////////////////////////////////////////////////////////////////////////////////////**

 **//为键盘安装事件通知关联，并准备获取采集**

 ** g_pKeyboard->SetEventNotification(g_Event);**

 ** g_pKeyboard->Acquire();**

 **//////////////////////////////////////////////////////////////////////////////////////**

 ** **

 ** **

 **//－－2－－**

 **/////////////////////////////////////////////////////////////////////////////////////////////**

 **//创建DirectInput8设备（鼠标），一下过程和键盘设置相同，不再注释**

 ** if( FAILED( hr = g_pDI->CreateDevice( GUID_SysMouse, &g_pMouse, NULL ) ) )**

 ** return hr;**

 ** if( FAILED( hr = g_pMouse->SetDataFormat( &c_dfDIMouse2 ) ) )**

 ** return hr;**

 ** hr = g_pMouse->SetCooperativeLevel( GetConsoleWindow(), DISCL_BACKGROUND | DISCL_NONEXCLUSIVE );**

 ** **

 ** dipdw.diph.dwSize = sizeof(DIPROPDWORD);**

 ** dipdw.diph.dwHeaderSize = sizeof(DIPROPHEADER);**

 ** dipdw.diph.dwObj = 0;**

 ** dipdw.diph.dwHow = DIPH_DEVICE;**

 ** dipdw.dwData = SAMPLE_BUFFER_SIZE;**

 ** **

 ** if( FAILED( hr = g_pMouse->SetProperty( DIPROP_BUFFERSIZE, &dipdw.diph ) ) )**

 ** return hr;**

 ** g_pMouse->SetEventNotification(g_Event);**

 ** g_pMouse->Acquire();**

 **//////////////////////////////////////////////////////////////////////////////////////**

 ** **

 ** **

 **//////////////////////////////////////////////////////////////////////////////////////**

 **//主线程**

 ** DWORD dwResult=0;**

 ** while(1)**

 ** {**

 ** dwResult = WaitForSingleObject(g_Event, INFINITE); //无限阻塞**

 ** **

 ** **

 ** ReadBufferedData_kb(); //读键盘数据并显示**

 ** ReadBufferedData_ms();//读鼠标数据并显示**

 ** }**

 **//////////////////////////////////////////////////////////////////////////////////////**

 **}**

 ** **

 **HRESULT ReadBufferedData_kb()**

 **{**

 ** TCHAR strNewText[256*5 + 1] = TEXT("");**

 ** TCHAR strLetter[50]; **

 ** DIDEVICEOBJECTDATA didod[ SAMPLE_BUFFER_SIZE ]; // Receives buffered data**

 ** DWORD dwElements;**

 ** DWORD i;**

 ** HRESULT hr;**

 ** **

 ** if( NULL == g_pKeyboard )**

 ** return S_OK;**

 ** **

 ** dwElements = SAMPLE_BUFFER_SIZE;**

 ** hr = g_pKeyboard->GetDeviceData( sizeof(DIDEVICEOBJECTDATA),**

 ** didod, &dwElements, 0 );**

 ** if( hr != DI_OK )**

 ** {**

 ** hr = g_pKeyboard->Acquire();**

 ** while( hr == DIERR_INPUTLOST )**

 ** hr = g_pKeyboard->Acquire();**

 ** **

 ** // Update the dialog text**

 ** if( hr == DIERR_OTHERAPPHASPRIO ||**

 ** hr == DIERR_NOTACQUIRED )**

 ** {**

 ** }**

 ** return S_OK;**

 ** }**

 ** **

 ** if( FAILED(hr) ) **

 ** return hr;**

 ** for( i = 0; i < dwElements; i++ )**

 ** {**

 ** wsprintf( strLetter, "0x%02x%s ", didod[ i ].dwOfs,**

 ** (didod[ i ].dwData & 0x80) ? "按下" : "抬起");**

 ** strcat( strNewText, strLetter );**

 ** }**

 ** printf("%s",strNewText);**

 ** **

 ** return S_OK;**

 **}**

 ** **

 ** **

 **HRESULT ReadBufferedData_ms()**

 **{**

 ** TCHAR strNewText[256*5 + 1] = TEXT("");**

 ** TCHAR strElement[50]; **

 ** DIDEVICEOBJECTDATA didod[ SAMPLE_BUFFER_SIZE ]; // Receives buffered data**

 ** DWORD dwElements;**

 ** DWORD i;**

 ** HRESULT hr;**

 ** **

 ** if( NULL == g_pMouse )**

 ** return S_OK;**

 ** **

 ** dwElements = SAMPLE_BUFFER_SIZE;**

 ** hr = g_pMouse->GetDeviceData( sizeof(DIDEVICEOBJECTDATA),**

 ** didod, &dwElements, 0 );**

 ** if( hr != DI_OK )**

 ** {**

 ** hr = g_pMouse->Acquire();**

 ** while( hr == DIERR_INPUTLOST )**

 ** hr = g_pMouse->Acquire();**

 ** **

 ** if( hr == DIERR_OTHERAPPHASPRIO ||**

 ** hr == DIERR_NOTACQUIRED )**

 ** {**

 ** }**

 ** return S_OK;**

 ** }**

 ** **

 ** if( FAILED(hr) ) **

 ** return hr;**

 ** for( i = 0; i < dwElements; i++ )**

 ** {**

 ** switch( didod[ i ].dwOfs )**

 ** {**

 ** case DIMOFS_BUTTON0:**

 ** strcpy( strElement, " 左键 " );**

 ** break;**

 ** **

 ** case DIMOFS_BUTTON1:**

 ** strcpy( strElement, " 右键 " );**

 ** break;**

 ** **

 ** case DIMOFS_BUTTON2:**

 ** strcpy( strElement, " 中键 " );**

 ** break;**

 ** **

 ** case DIMOFS_BUTTON3:**

 ** strcpy( strElement, "B3" );**

 ** break;**

 ** **

 ** case DIMOFS_X:**

 ** strcpy( strElement, " 水平 " );**

 ** break;**

 ** **

 ** case DIMOFS_Y:**

 ** strcpy( strElement, " 垂直 " );**

 ** break;**

 ** **

 ** case DIMOFS_Z:**

 ** strcpy( strElement, " 滚动 " );**

 ** break;**

 ** **

 ** default:**

 ** strcpy( strElement, "" );**

 ** }**

 ** **

 ** switch( didod[ i ].dwOfs )**

 ** {**

 ** case DIMOFS_BUTTON0:**

 ** case DIMOFS_BUTTON1:**

 ** case DIMOFS_BUTTON2:**

 ** case DIMOFS_BUTTON3:**

 ** if( didod[ i ].dwData & 0x80 )**

 ** strcat( strElement, " 抬起 " );**

 ** else**

 ** strcat( strElement, " 按下 " );**

 ** break;**

 ** **

 ** case DIMOFS_X:**

 ** case DIMOFS_Y:**

 ** case DIMOFS_Z:**

 ** {**

 ** TCHAR strCoordValue[255];**

 ** wsprintf( strCoordValue, "%d ", didod[ i ].dwData );**

 ** strcat( strElement, strCoordValue );**

 ** break;**

 ** }**

 ** }**

 ** strcat( strNewText, strElement );**

 ** }**

 ** printf("%s/n",strNewText);**

 ** return S_OK;**

 **}**

 ** **   
 