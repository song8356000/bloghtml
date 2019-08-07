---
title: RtlZeroMemory
date: 2016-06-20 19:29:05
tags: CSDN迁移
---
   **ZeroMemory  
 ZeroMemory宏  
  
  
 The ZeroMemory macro fills a block of memory with zeros.  
 ZeroMemory宏用0来填充一块内存区域。  
 To avoid undesired effects of optimizing compilers, use the SecureZeroMemory function.  
 为了避免优化编译器的意外的影响，请使用SecureZeroMemory函数。  
  
  
 void ZeroMemory(  
 PVOID Destination, //地址  
 SIZE_T Length //填0的长度  
 );  
  
  
 Parameters  
 参数：  
 Destination   
 [in] Pointer to the starting address of the block of memory to fill with zeros.   
 指向一块准备用0来填充的内存区域的开始地址。  
 Length   
 [in] Size of the block of memory to fill with zeros, in bytes.   
 准备用0来填充的内存区域的大小，按字节来计算。  
  
  
 Return Values  
 返回值：  
 This function has no return value.  
 这个函数没有返回值。  
  
  
 Remarks  
 备注：  
 This function is defined as the RtlZeroMemory macro. For more information, see Winbase.h and Winnt.h.  
 这个函数被定义为RtlZeroMemory宏。至于更多信息，请查看Winbase.h与Winnt.h这两个头文件。  
  
  
 Requirements  
 必要条件：  
 Client: Requires Windows XP, Windows 2000 Professional, Windows NT Workstation, Windows Me, Windows 98, or Windows 95.  
 Server: Requires Windows Server 2003, Windows 2000 Server, or Windows NT Server.  
 Header: Declared in Winbase.h; include Windows.h.  
  
  
 See Also  
 请参阅  
 Memory Management Overview, Memory Management Functions, CopyMemory, FillMemory, MoveMemory, SecureZeroMemory**   
 