---
title: 汇编语言通过WMI获取BIOS、主板、硬盘、CPU、网卡的信息
date: 2017-04-13 15:55:39
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/70158726   
   前几天在网上看见某大牛使用汇编语言写的通过WMI获取BIOS,主板，硬盘，CPU，网卡的信息的一篇文章，发现真是写的太棒了，最近正好想要用汇编写点东西，就拿着作者的源码修改了点东西，来实现自己的需求，我是在RadASM下编译使用的，大牛的解决方法正好给我提供了许多思路，也让我学到了很多，下面就是我修改后的源代码：


```
.586

.MODEL FLAT,STDCALL

OPTION CASEMAP:NONE
INCLUDE windows.inc

INCLUDE kernel32.inc
INCLUDELIB kernel32.lib

INCLUDE ole32.inc
INCLUDELIB ole32.lib

INCLUDE user32.inc
INCLUDELIB user32.lib

INCLUDE masm32.inc
INCLUDELIB masm32.lib

GetWmiInfo proto :LPWSTR, :LPSTR, :LPSTR

g_debug equ 0

; located in ObjIdl.h

EOAC_NONE   EQU 0
COINIT_MULTITHREADED equ 00h

; located in RpcDce.h

RPC_C_AUTHN_LEVEL_DEFAULT   EQU 0
RPC_C_IMP_LEVEL_DEFAULT     EQU 0
RPC_C_IMP_LEVEL_IMPERSONATE EQU 3

GUID2 STRUCT
     dd1 DWORD ?
     dw1 WORD ?
     dw2 WORD ?
     db1 BYTE ?
     db2 BYTE ?
     db3 BYTE ?
     db4 BYTE ?
     db5 BYTE ?
     db6 BYTE ?
     db7 BYTE ?
     db8 BYTE ?
GUID2 ENDS

IWbemLocator STRUCT
    lpVtbl DWORD   ?
IWbemLocator ENDS


IWbemLocatorVtbl STRUCT

    QueryInterface DWORD   ?
    AddRef         DWORD   ?
    Release        DWORD   ?
    ConnectServer  DWORD   ?
IWbemLocatorVtbl ENDS

IWbemServices STRUCT
    lpVtbl DWORD   ?
IWbemServices ENDS

IWbemServicesVtbl STRUCT

    QueryInterface             DWORD   ?
    AddRef                     DWORD   ?
    Release                    DWORD   ?
    OpenNamespace              DWORD   ?
    CancelAsyncCall            DWORD   ?
    QueryObjectSink            DWORD   ?
    GetObject                  DWORD   ?
    GetObjectAsync             DWORD   ?
    PutClass                   DWORD   ?
    PutClassAsync              DWORD   ?
    DeleteClass                DWORD   ?
    DeleteClassAsync           DWORD   ?
    CreateClassEnum            DWORD   ?
    CreateClassEnumAsync       DWORD   ?
    PutInstance                DWORD   ?
    PutInstanceAsync           DWORD   ?
    DeleteInstance             DWORD   ?
    DeleteInstanceAsync        DWORD   ?
    CreateInstanceEnum         DWORD   ?
    CreateInstanceEnumAsync    DWORD   ?
    ExecQuery                  DWORD   ?
    ExecQueryAsync             DWORD   ?
    ExecNotificationQuery      DWORD   ?
    ExecNotificationQueryAsync DWORD   ?
    ExecMethod                 DWORD   ?
    ExecMethodAsync            DWORD   ?

IWbemServicesVtbl ENDS



IEnumWbemClassObject STRUCT
    lpVtbl          DWORD   ?
IEnumWbemClassObject ENDS

IEnumWbemClassObjectVtbl STRUCT

    QueryInterface DWORD   ?
    AddRef         DWORD   ?
    Release        DWORD   ?
    Reset          DWORD   ?
    Next           DWORD   ?
    NextAsync      DWORD   ?
    Clone          DWORD   ?
    Skip           DWORD   ?
IEnumWbemClassObjectVtbl ENDS

IWbemClassObject STRUCT
    lpVtbl DWORD   ?
IWbemClassObject ENDS

IWbemClassObjectVtbl STRUCT
    QueryInterface          DWORD   ?
    AddRef                  DWORD   ?
    Release                 DWORD   ?
    GetQualifierSet         DWORD   ?
    Get                     DWORD   ?
    Put                     DWORD   ?
    Delete                  DWORD   ?
    GetNames                DWORD   ?
    BeginEnumeration        DWORD   ?
    Next                    DWORD   ?
    EndEnumeration          DWORD   ?
    GetPropertyQualifierSet DWORD   ?
    GetObjectText           DWORD   ?
    SpawnDerivedClass       DWORD   ?
    SpawnInstance           DWORD   ?
    CompareTo               DWORD   ?
    GetPropertyOrigin       DWORD   ?
    InheritsFrom            DWORD   ?
    GetMethod               DWORD   ?
    PutMethod               DWORD   ?
    DeleteMethod            DWORD   ?
    BeginMethodEnumeration  DWORD   ?
    NextMethod              DWORD   ?
    EndMethodEnumeration    DWORD   ?
    GetMethodQualifierSet   DWORD   ?
    GetMethodOrigin         DWORD   ?
IWbemClassObjectVtbl ENDS


SAFEARRAYBOUND struct

    cElements   dd ?    ;这一维有多少个元素？
    lLbound     dd ?    ;它的索引从几开始？
SAFEARRAYBOUND ends

 
SAFEARRAY struct

    cDims       dw ?    ;Count of dimensions in this array.这个数组有几维？
    fFeatures   dw ?    ;Flags used by the SafeArray routines documented below. 数组有什么特性？
    cbElements  dd ?    ;Size of an element of the array. Does not include size of pointed-to data.
                        ;数组的每个元素有多大?
    cLocks      dd ?    ;Number of times the array has been  locked without corresponding unlock.
                        ;这个数组被锁定过几次？
    pvData      dd ?    ;Pointer to the data. 数组里的数据放在什么地方？
    rgsabound   SAFEARRAYBOUND <> ;One bound for each dimension.真数组
SAFEARRAY ends

;ssssssssssssssssssssssss

.DATA

;ssssssssssssssssssssssss
    g_wszSelect WORD "S","E","L","E","C","T"," ","*"," ","F","R","O","M"," ", 0
    g_szBiosVerion    db 0dh, 0ah, "BIOS版本信息：", 0
    g_wszWin32_BIOS   word  "W", "i", "n", "3", "2", "_", "B", "I", "O", "S", 0
    g_wszBIOSVerstion word  "B", "I", "O", "S", "V", "e", "r", "s", "i", "o", "n", 0
    g_szBiosSerialNumber db 0dh, 0ah, "BIOS序列号：", 0
    g_szHDDSerialNum db 0dh, 0ah, "硬盘序列号：", 0
    g_wszWin32_PhysicalMedia word "W", "i", "n", "3", "2", "_"
                WORD "P", "h", "y", "s", "i", "c", "a", "l", "M", "e", "d", "i", "a", 0
    g_wszSerialNumber word "S", "e", "r", "i", "a", "l", "N", "u", "m", "b", "e", "r", 0
    g_szBaseBoardSerialNum db 0dh, 0ah, "主板序列号：", 0
    g_wszWin32_BaseBoard word "W", "i", "n", "3", "2", "_", "B", "a", "s", "e", "B", "o", "a", "r", "d", 0
    g_szCpuId db 0dh, 0ah, "CPU ID：", 0
    g_wszWin32_Processor word "W", "i", "n", "3", "2", "_", "P", "r", "o", "c", "e", "s", "s", "o", "r", 0
    g_wszProcessorId word "P", "r", "o", "c", "e", "s", "s", "o", "r", "I", "d", 0
    g_szNidMac db 0dh, 0ah, "网卡 MAC：", 0
    g_wszWin32_NetworkAdapter word "W", "i", "n", "3", "2", "_"

                WORD "N", "e", "t", "w", "o", "r", "k", "A", "d", "a", "p", "t", "e", "r", 0
    g_wszMACAddress word "M", "A", "C", "A", "d", "d", "r", "e", "s", "s", 0
    g_wszNameSpace word "r", "o", "o", "t", "\", "c", "i", "m", "v", "2", 0
    g_wszQueryLanguage word "W", "Q", "L", 0
    g_szAppInfo db "通过WMI获取硬件信息", 0dh ,0ah
                db "小哈龙", 0dh ,0ah, 0
    g_szPerSCr db "%S"
    g_szCrLf db 0dh, 0ah, 0
    g_szFail db "Fail", 0
    szone db "我的测试",0
    szoption db "标题",0

    ; located in WbemCli.h

    WBEM_FLAG_CONNECT_USE_MAX_WAIT  EQU     80h
    WBEM_FLAG_FORWARD_ONLY          EQU     20h
    WBEM_INFINITE                   EQU     -1
    WBEM_E_INVALID_QUERY            EQU     80041017h
    WBEM_E_INVALID_QUERY_TYPE       EQU     80041018h

    IID_IWbemLocator                GUID2   <0dc12a687h,0737fh,011cfh,088h,04dh,000h,0aah,000h,04bh,02eh,024h>
    IID_IEnumWbemClassObject        GUID2   <027947e1h,0d731h,011ceh,0a3h,057h,000h,000h,000h,000h,000h,001h>
    IID_IWbemClassObject            GUID2   <0dc12a681h,0737fh,011cfh,088h,04dh,000h,0aah,000h,04bh,02eh,024h>

    ; located in WbemProv.h

    CLSID_WbemAdministrativeLocator GUID2   <0cb8555cch,09128h,011d1h,0adh,09bh,000h,0c0h,04fh,0d8h,0fdh,0ffh>

    locator     IWbemLocator            <>
    service     IWbemServices           <>
    enumerator  IEnumWbemClassObject    <>
    processor   IWbemClassObject        <>

    retCount    DWORD   ?

    var_val     DWORD   ?
                DWORD   ?
                DWORD   ?
                DWORD   ?
    wszQuery   WORD 256 dup(?)
    g_szBuf512 byte 512 dup(?)
;ssssssssssssssssssssssss
.CODE
;ssssssssssssssssssssssss

start:
    invoke CoInitializeEx, NULL, COINIT_MULTITHREADED
    invoke CoInitializeSecurity, NULL, -1, NULL, NULL, RPC_C_AUTHN_LEVEL_DEFAULT,\
                RPC_C_IMP_LEVEL_IMPERSONATE, NULL, EOAC_NONE, NULL
    invoke CoCreateInstance, ADDR CLSID_WbemAdministrativeLocator, NULL,\
                CLSCTX_INPROC_SERVER, ADDR IID_IWbemLocator, ADDR locator
    invoke StdOut, ADDR g_szAppInfo
    invoke StdOut, ADDR g_szBiosVerion
    mov    byte ptr g_szBuf512, NULL
    invoke GetWmiInfo, ADDR g_wszWin32_BIOS, ADDR g_wszBIOSVerstion, ADDR g_szBuf512
    .if byte ptr g_szBuf512 != NULL
        invoke StdOut, ADDR g_szBuf512
        invoke MessageBox,NULL,addr g_szBuf512,offset g_szBiosVerion,MB_OK
    .else
        invoke StdOut, ADDR g_szFail 
    .endif
    invoke StdOut, ADDR g_szBiosSerialNumber
    mov    byte ptr g_szBuf512, NULL
    invoke GetWmiInfo, ADDR g_wszWin32_BIOS, ADDR g_wszSerialNumber, ADDR g_szBuf512
    .if byte ptr g_szBuf512 != NULL

        invoke StdOut, ADDR g_szBuf512
    .else
        invoke StdOut, ADDR g_szFail 
    .endif
    invoke StdOut, ADDR g_szHDDSerialNum
    mov    byte ptr g_szBuf512, NULL
    invoke GetWmiInfo, ADDR g_wszWin32_PhysicalMedia, ADDR g_wszSerialNumber, ADDR g_szBuf512
    .if byte ptr g_szBuf512 != NULL 
        invoke StdOut, ADDR g_szBuf512
        invoke MessageBox,NULL,offset g_szBuf512,offset g_szHDDSerialNum,MB_OK
    .else
        invoke StdOut, ADDR g_szFail
    .endif
    invoke StdOut, ADDR g_szBaseBoardSerialNum
    mov    byte ptr g_szBuf512, NULL
    invoke GetWmiInfo, ADDR g_wszWin32_BaseBoard, ADDR g_wszSerialNumber, ADDR g_szBuf512
    .if byte ptr g_szBuf512 != NULL
        invoke MessageBox,NULL,addr g_szBuf512,offset g_szBaseBoardSerialNum,MB_OK
        invoke StdOut, ADDR g_szBuf512
    .else
        invoke StdOut, ADDR g_szFail
    .endif
    invoke StdOut, ADDR g_szCpuId
    mov    byte ptr g_szBuf512, NULL
    invoke GetWmiInfo, ADDR g_wszWin32_Processor, ADDR g_wszProcessorId, ADDR g_szBuf512
    .if byte ptr g_szBuf512 != NULL
        invoke StdOut, ADDR g_szBuf512
        invoke MessageBox,NULL,offset g_szBuf512,offset g_szCpuId,MB_OK
    .else
        invoke StdOut, ADDR g_szFail
    .endif
    invoke StdOut, ADDR g_szNidMac
    mov    byte ptr g_szBuf512, NULL
    invoke GetWmiInfo, ADDR g_wszWin32_NetworkAdapter, ADDR g_wszMACAddress, ADDR g_szBuf512
    .if byte ptr g_szBuf512 != NULL
        invoke StdOut, ADDR g_szBuf512
        invoke MessageBox,NULL,offset g_szBuf512,offset g_szNidMac,MB_OK
    .else
        invoke StdOut, ADDR g_szFail
    .endif
    invoke CoUninitialize
    invoke ExitProcess, 0

GetWmiInfo proc lpwszType: LPWSTR, lpwszItem: LPSTR, lpszBuf: LPSTR

    LOCAL wszbuf[256]: word
    invoke lstrcpyW, ADDR wszQuery, ADDR g_wszSelect
    invoke lstrcatW, ADDR wszQuery, lpwszType
    mov esi, locator
    lodsd
    push    OFFSET service
    push    NULL
    push    NULL
    push    WBEM_FLAG_CONNECT_USE_MAX_WAIT
    push    NULL
    push    NULL
    push    NULL
    push    OFFSET g_wszNameSpace
    push    DWORD PTR [locator]
    call    DWORD PTR [eax][IWbemLocatorVtbl.ConnectServer]
    mov esi, service
    lodsd
    push    OFFSET enumerator
    push    NULL
    push    WBEM_FLAG_FORWARD_ONLY
    push    OFFSET wszQuery
    push    OFFSET g_wszQueryLanguage
    push    DWORD PTR [service]
    call    DWORD PTR [eax][IWbemServicesVtbl.ExecQuery]
    mov esi, enumerator
    lodsd
    push    OFFSET retCount
    push    OFFSET processor
    push    TRUE
    push    WBEM_INFINITE
    push    DWORD PTR [enumerator]
    call    DWORD PTR [eax][IEnumWbemClassObjectVtbl.Next]
    mov esi, processor
    lodsd
    push    NULL
    push    NULL
    push    OFFSET var_val
    push    0
    push    lpwszItem
    push    DWORD PTR [processor]
    call    DWORD PTR [eax][IWbemClassObjectVtbl.Get]
if g_debug eq 1
    jmp @F
    g_sz1 db 0dh, 0ah, "eax=%d, ecx=%x, esi=%x, edi=%x", 0dh, 0ah, 0
@@:
    mov esi, [var_val]
    mov edi, [var_val + 4]
    mov ecx, [var_val + 8]0
    mov eax, [var_val + 12]
    pushad
    invoke wsprintf, ADDR wszbuf, ADDR g_sz1, eax, ecx, esi, edi
    invoke StdOut, ADDR wszbuf
    popad
endif   ;g_debug eq 1
    mov eax, [var_val]
    test eax, VT_BSTR
    .if !ZERO?
        test eax, VT_ARRAY
        .IF !ZERO?
            mov ecx, [var_val + 8]
            mov esi,[ecx].SAFEARRAY.pvData
            mov edi,[ecx].SAFEARRAY.rgsabound.cElements
if g_debug eq 1
            movzx eax, [ecx].SAFEARRAY.cDims
            mov ecx,[ecx].SAFEARRAY.rgsabound.lLbound
            pushad
            invoke wsprintf, ADDR wszbuf, ADDR g_sz1, eax, ecx, esi, edi
            invoke StdOut, ADDR wszbuf
            popad
endif   ;g_debug eq 1
            .repeat ; while edi
                push esi
                push edi
                mov ecx, [esi]
                invoke wsprintf, ADDR wszbuf, ADDR g_szPerSCr, ecx
                invoke lstrcat, lpszBuf, ADDR wszbuf
                pop edi
                pop esi
                dec edi
                add esi,4
            .until edi==0 ;endw
        .ELSE
                invoke wsprintf, ADDR wszbuf, ADDR g_szPerSCr, [var_val + 8]
                invoke lstrcat, lpszBuf, ADDR wszbuf
        .ENDIF
    .endif
    ret
GetWmiInfo endp
;======================================================
END start

```
  
   
 