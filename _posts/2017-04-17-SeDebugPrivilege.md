---
title: SeDebugPrivilege
date: 2017-04-10 20:16:44
tags: CSDN迁移
---
   要对一个任意进程（包括系统安全进程和服务进程）进行指定了写相关的访问权的OpenProcess操作，  
 只要当前进程具有SeDeDebug权限就可以了。  
 要是一个用户是Administrator或是被给予了相应的权限，  
 就可以具有该权限。  
 可是，就算我们用Administrator帐号对一个系统安全进程执行OpenProcess(PROCESS_ALL_ACCESS,FALSE, dwProcessID)  
 还是会遇到“访问拒绝”的错误。  
 什么原因呢？原来在默认的情况下进程的一些访问权限是没有被使能（Enabled）的，  
 所以我们要做的首先是使能这些权限。  
 与此相关的一些API函数有OpenProcessToken、LookupPrivilegevalue、AdjustTokenPrivileges。  
 我们要修改一个进程的访问令牌，首先要获得进程访问令牌的句柄，这可以通过OpenProcessToken得到，函数的原型如下：  
 函数原形如下：  
 BOOL OpenProcessToken(  
 HANDLE ProcessHandle, //要修改访问权限的进程句柄；  
 DWORD DesiredAccess, //指定你所需要的操作类型；  
 PHANDLE TokenHandle //返回的访问令牌指针  
 );  
 第一参数是要修改访问权限的进程句柄；第三个参数就是返回的访问令牌指针；第二个参数指定你要进行的操作类型，如要修改令牌我们要指定第二个参数为TOKEN_ADJUST_PRIVILEGES（其它一些参数可参考Platform SDK）。通过这个函数我们就可以得到当前进程的访问令牌的句柄（指定函数的第一个参数为GetCurrentProcess()就可以了）。接着我们可以调用AdjustTokenPrivileges对这个访问令牌进行修改。AdjustTokenPrivileges的原型如下：  
 BOOL AdjustTokenPrivileges(  
 HANDLE TokenHandle, // handle to token  
 BOOL DisableAllPrivileges, // disabling option  
 PTOKEN_PRIVILEGES NewState, // privilege information  
 DWORD BufferLength, // size of buffer  
 PTOKEN_PRIVILEGES PreviousState, // original state buffer  
 PDWORD ReturnLength // required buffer size  
 );  
 第一个参数是访问令牌的句柄；第二个参数决定是进行权限修改还是除能（Disable）所有权限；第三个参数指明要修改的权限，是一个指向TOKEN_PRIVILEGES结构的指针，该结构包含一个数组，数据组的每个项指明了权限的类型和要进行的操作; 第四个参数是结构PreviousState的长度，如果PreviousState为空，该参数应为NULL；第五个参数也是一个指向TOKEN_PRIVILEGES结构的指针，存放修改前的访问权限的信息，可空；最后一个参数为实际PreviousState结构返回的大小。在使用这个函数前再看一下TOKEN_PRIVILEGES这个结构，其声明如下：  
 ypedef struct _TOKEN_PRIVILEGES {   
 DWORD PrivilegeCount;   
 LUID_AND_ATTRIBUTES Privileges[];   
 } TOKEN_PRIVILEGES, *PTOKEN_PRIVILEGES;   
 PrivilegeCount指的数组原素的个数，接着是一个LUID_AND_ATTRIBUTES类型的数组，再来看一下LUID_AND_ATTRIBUTES这个结构的内容，声明如下：  
 typedef struct _LUID_AND_ATTRIBUTES {   
 LUID Luid;   
 DWORD Attributes;   
 } LUID_AND_ATTRIBUTES, *PLUID_AND_ATTRIBUTES  
 第二个参数就指明了我们要进行的操作类型，有三个可选项： SE_PRIVILEGE_ENABLED、SE_PRIVILEGE_ENABLED_BY_DEFAULT、SE_PRIVILEGE_USED_FOR_ACCESS。要使能一个权限就指定Attributes为SE_PRIVILEGE_ENABLED。第一个参数就是指权限的类型，是一个LUID的值，LUID就是指locally unique identifier，我想GUID大家是比较熟悉的，和GUID的要求保证全局唯一不同，LUID只要保证局部唯一，就是指在系统的每一次运行期间保证是唯一的就可以了。另外和GUID相同的一点，LUID也是一个64位的值，相信大家都看过GUID那一大串的值，我们要怎么样才能知道一个权限对应的LUID值是多少呢？这就要用到另外一个API函数LookupPrivilegevalue，其原形如下：  
 BOOL LookupPrivilegevalue(  
 LPCTSTR lpSystemName, // system name  
 LPCTSTR lpName, // privilege name  
 PLUID lpLuid // locally unique identifier  
 );  
 第一个参数是系统的名称，如果是本地系统只要指明为NULL就可以了，第三个参数就是返回LUID的指针，第二个参数就是指明了权限的名称，如“SeDebugPrivilege”。在Winnt.h中还定义了一些权限名称的宏，如：  
 #define SE_BACKUP_NAME TEXT("SeBackupPrivilege")  
 #define SE_RESTORE_NAME TEXT("SeRestorePrivilege")  
 #define SE_SHUTDOWN_NAME TEXT("SeShutdownPrivilege")  
 #define SE_DEBUG_NAME TEXT("SeDebugPrivilege")  
 这样通过这三个函数的调用，我们就可以用OpenProcess(PROCESS_ALL_ACCESS,FALSE, dwProcessID)来打获得任意进程的句柄，并且指定了所有的访问权。   
 