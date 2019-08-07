---
title: C++实现CString和string的互相转换
date: 2017-07-03 19:08:14
tags: CSDN迁移
---
   CString->std::string 例子：   
  
 CString strMfc=“test“;   
  
 std::string strStl;   
  
 strStl=strMfc.GetBuffer(0);

 unicode情形下：

 CStringW strw = _T("test");  
 CStringA stra(strw.GetBuffer(0));  
 strw.ReleaseBuffer();

 std::string imgpath=stra.GetBuffer(0);  
 stra.ReleaseBuffer();  
  
 std::string->CString 例子：   
  
 CString strMfc；   
  
 std::string strStl=“test“;   
  
 strMfc=strStl.c_str();   
  


 AfxExtractSubString是截取字符串的函数，很好用，不过美中不足的地方在与它只能使用单个字符作为分割符。

 但是这种情况在很多时候都行不通，如果分割符需要是两个字符以上呢？

 之前因为这个问题试了很久，也在网上搜索过。不过可惜的是，网上的大部分关于VC截取字符串的文章都是那么同样的几篇，都是写的满复杂然后可以实现了AfxExtractSubString功能而已的，也就是只能用单个字符截取，但是标题却写着用字符串截取字符串，好笑！

 不找了，自己写吧。CString里面有Find，然后再组成数组。

 ![](http://images.csdn.net/syntaxhighlighting/outliningindicators/none.gif)void Split(CString source, CStringArray& dest, CString division)  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/expandedblockstart.gif){  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/inblock.gif) dest.RemoveAll();  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/inblock.gif) int pos = 0;  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/inblock.gif) int pre_pos = 0;  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/expandedsubblockstart.gif) while( -1 != pos ){  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/inblock.gif) pre_pos = pos;  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/inblock.gif) pos = source.Find(division,(pos+1));  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/inblock.gif) dest.Add(source.Mid(pre_pos,(pos-pre_pos)));  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/expandedsubblockend.gif) }  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/inblock.gif)  
![](http://images.csdn.net/syntaxhighlighting/outliningindicators/expandedblockend.gif)}  
 CString source是需要截取的原字符串，

 CStringArray& dest 是最终结果的数组

 CString division 是用来做分割符的字符串

 

 备忘：为了适用于Unicode环境，要养成使用_T()宏的习惯

 1、格式化字符串

 CString s;  
 s.Format(_T("The num is %d."), i);

 

 2、转为 int

 转10进制最好用_ttoi()，它在 ANSI 编码系统中被编译成_atoi()，而在 Unicode 编码系统中编译成_wtoi()。用_tcstoul()或者_tcstol()可以把字符串转化成任意进制的（无符号/有符号）长整数。

 CString hex = _T("FAB");  
 CString decimal = _T("4011");  
 ASSERT(_tcstoul(hex, 0, 16) == _ttoi(decimal));

 

 3、转为 char *

 3.1 强制类型转换为 LPCTSTR，不能修改字符串

 LPCTSTR p = s; 或者直接 (LPCTSTR)s;

 3.2 使用 GetBuffer 方法

 不给 GetBuffer 传递参数时它使用默认值 0，意思是：“给我这个字符串的指针，我保证不加长它”。假设你想增加字符串的长度，就必须将你需要的字符空间大小（注意：是字符而不是字节，因为 CString 是以隐含方式感知 Unicode 的）传给它。当调用 ReleaseBuffer 时，字符串的实际长度会被重新计算，然后存入 CString 对象中。  
 必须强调一点，在 GetBuffer 和 ReleaseBuffer 之间这个范围，一定不能使用你要操作的这个缓冲的 CString 对象的任何方法。因为 ReleaseBuffer 被调用之前，该 CString 对象的完整性得不到保障。

 LPTSTR p = s.GetBuffer();  
 // do something with p  
int m = s.GetLength(); // 可能出错！！！  
s.ReleaseBuffer();  
 int n = s.GetLength(); // 保证正确

 4、其他

 4.1 分割字符串

 AfxExtractSubString(CString& rString, LPCTSTR lpszFullString, int iSubString, TCHAR chSep = '/n');

 CString csFullString(_T("abcd-efg-hijk-lmn"));  
 CString csTemp;  
 AfxExtractSubString(csTemp, (LPCTSTR)csFullString, 0, '-'); // 得到 abcd  
 AfxExtractSubString(csTemp, (LPCTSTR)csFullString, 1, '-'); // 得到 efg  
 AfxExtractSubString(csTemp, (LPCTSTR)csFullString, 2, '-'); // 得到 hijk  
 AfxExtractSubString(csTemp, (LPCTSTR)csFullString, 3, '-'); // 得到 lmn

 分隔符可以随便指定：  
 AfxExtractSubString(csTemp, (LPCTSTR)csFullString, 0, 'f'); // 得到 abcd-e

   
   
 