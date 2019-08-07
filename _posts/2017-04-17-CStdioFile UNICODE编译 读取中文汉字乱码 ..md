---
title: CStdioFile UNICODE编译 读取中文汉字乱码 .
date: 2018-12-29 16:07:02
tags: CSDN迁移
---
   函数原形为：char *setlocale( int category, const char *locale );  
 头文件：<locale.h>  
 所支持的操作系统为:ANSI, Win 95, Win NT  
 对于简体中文可以使用如下设置：setlocale( LC_ALL, "chs" );   
 为什么一定要调用setlocale呢？  
 因为在C/C++语言标准中定义了其运行时的字符集环境为"C"，也就是ASCII字符集的一个子集，那么mbstowcs在工作时会将cstr中所包含的字符串看作是ASCII编码的字符，而不认为是一个包含有chs编码的字符串，所以他会将每一个中文拆成2个ASCII编码进行转换，这样得到的结果就是会形成4个wchar_t的字符组成的串，那么如何才能够让mbstowcs正常工作呢？在调用mbstowcs进行转换之间必须明确的告诉mbstowcs目前cstr串中包含的是chs编码的字符串，通过setlocale( LC_ALL, "chs" )函数调用来完成，需要注意的是这个函数会改变整个应用程序的字符集编码方式，必须要通过重新调用setlocale( LC_ALL, "C" )函数来还原，这样就可以保证mbstowcs在转换时将cstr中的串看作是中文串，并且转换成为2个wchar_t字符，而不是4个。

 本地化设置需要具备三个条件：  
 a. 语言代码 (Language Code)  
 b. 国家代码 (Country Code)   
 c. 编码(Encoding)  
 本地名字可以用下面这些部分来构造：  
 语言代码_国家代码.编码 比如（zh_CN.UTF-8, en_US等）

 
```
 CStdioFile file 
//设置语言为中文,否则在Unicode编码下读出中文字符为乱码 
char* old_locale=_strdup( setlocale(LC_CTYPE,NULL) ); 
setlocale( LC_CTYPE,"chs"); 
file.Open( strOutputFile,CStdioFile::modeRead); 
file.Read(); 
file.Close(); 
setlocale( LC_CTYPE, old_locale ); //还原语言区域的设置 
free( old_locale );//还原区域设定


```
 用下面的也行

 
```
 CStdioFile   file1; 
file1.Open(_T( "f:\\jun\\kj66\\dev.txt "),CFile::modeRead); 
CString   strTem(_T( " ")); 
file1.ReadString(strTem); 
file1.ReadString(strTem); 
int   n   =   strTem.GetLength(); 
TCHAR*   p   =   strTem.GetBuffer(); 
char*   pp   =   new   char[n+1]; 
int   j   =   0; 
while(n) 
{ 
*(pp+j)   =   (char)   *(p+j); 
n--; 
j++; 
} 
*(pp+j)   =   '\0 '; 
strTem   =   pp; 
MessageBoxW(strTem); 
file1.Close(); 
```
 本文转载自：[https://www.cnblogs.com/ct0421/p/3242418.html](https://www.cnblogs.com/ct0421/p/3242418.html)

   
 