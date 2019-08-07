---
title: C#如何运行外部程序（打开可执行程序）：ShellExcute和Process
date: 2017-09-13 10:57:31
tags: CSDN迁移
---
   最近的任务用到C#来调用C++内核程序，也就是C++编译运行后生成的.exe文件。网搜了一下C#中运行外部程序大致有两种方法，在此稍作总结：

 （1）使用API函数ShellExcute

 添加引用 using System.Runtime.InteropServices;

 public enum ShowWindowCommands : int  
 {  
  
 SW_HIDE = 0,  
 SW_SHOWNORMAL = 1, //用最近的大小和位置显示，激活  
 SW_NORMAL = 1,  
 SW_SHOWMINIMIZED = 2,  
 SW_SHOWMAXIMIZED = 3,  
 SW_MAXIMIZE = 3,  
 SW_SHOWNOACTIVATE = 4,  
 SW_SHOW = 5,  
 SW_MINIMIZE = 6,  
 SW_SHOWMINNOACTIVE = 7,  
 SW_SHOWNA = 8,  
 SW_RESTORE = 9,  
 SW_SHOWDEFAULT = 10,  
 SW_MAX = 10  
 }  
  [DllImport("shell32.dll")]  
 public static extern IntPtr ShellExecute(  
 IntPtr hwnd,  
 string lpszOp,  
 string lpszFile,  
 string lpszParams,  
 string lpszDir,  
 ShowWindowCommands FsShowCmd  
 );

 执行语句只有这句：  


 ShellExecute(IntPtr.Zero, "open", "testIO.exe", null, null, ShowWindowCommands.SW_SHOWNORMAL);

 对于ShellExecute函数，[http://baike.baidu.com/link?url=j6AhrfaS5YyMQ_8peNIdCsM0SWCjMoVDK_Lbzi5A4lz7VjNNCRsZU-bXrYFgfe6T_Rd1MKjQ9GelkuolNqNx1K](http://baike.baidu.com/link?url=j6AhrfaS5YyMQ_8peNIdCsM0SWCjMoVDK_Lbzi5A4lz7VjNNCRsZU-bXrYFgfe6T_Rd1MKjQ9GelkuolNqNx1K)

 

 //调用计算器  
 ShellExecute(NULL,"open","calc.exe",NULL,NULL,SW_SHOWNORMAL);  
 //调用[记事本](http://baike.baidu.com/view/152865.htm)  
 ShellExecute(NULL,"open","NOTEPAD.EXE",NULL,NULL,SW_SHOWNORMAL);  
 ●hWnd：用于指定父[窗口句柄](http://baike.baidu.com/view/1452762.htm)。当[函数调用](http://baike.baidu.com/view/2369016.htm)过程出现错误时，它将作为Windows消息窗口的父窗口。例如，可以将其设置为[应用程序](http://baike.baidu.com/view/330120.htm)主[窗口句柄](http://baike.baidu.com/view/1452762.htm)，即Application.Handle，也可以将其设置为[桌面](http://baike.baidu.com/view/79807.htm)窗口句柄（用GetDesktopWindow函数获得）。  
 ●Operation：用于指定要进行的操作。其中“open”操作表示执行由FileName参数指定的程序，或打开由FileName参数指定的文件或文件夹；“print”操作表示打印由FileName参数指定的文件；“explore”操作表示浏览由FileName参数指定的文件夹。当参数设为nil时，表示执行默认操作“open”。  
 ●FileName：用于指定要打开的文件名、要执行的程序文件名或要浏览的文件夹名。  
 ●Parameters：若FileName参数是一个可执行程序，则此参数指定[命令行参数](http://baike.baidu.com/view/1646320.htm)，否则此参数应为nil或PChar(0)。  
 ●Directory：用于指定默认目录。  
 ●ShowCmd：若FileName参数是一个可执行程序，则此参数指定程序窗口的初始显示方式，否则此参数应设置为0。  
 若ShellExecute[函数调用](http://baike.baidu.com/view/2369016.htm)成功，则返回值为被执行程序的实例句柄。若返回值小于32，则表示出现错误。  
  另外，ShellExecute还有一些特殊用法，如打开网页，打开邮件窗口等，详细内容看参看上面的链接。  
 （2）使用Process类

 添加引用using System.Diagnostics;

 ** **Process process = new Process();  
 //process.StartInfo.UseShellExecute = false;  
 process.StartInfo.FileName = "testIO.exe";  
 //process.StartInfo.CreateNoWindow = true;  
  process.Start();

 如此即可用C#打开testIO.exe文件。。。。。。

   
 