---
title: CString中 format、trimLeft和trimright、trim 和FindOneOf用法
date: 2018-12-03 15:35:17
tags: CSDN迁移
---
   1.format 可以帮助各种类型转换成CString.

 a. int 转 CString 

 CString str;

 int number = 4;

 str.Format(_T("%d"),number);

 b. double 转 CString 

 CString str;

 double num = 1.46;

 str.Format(_T("%lf"),num);

 c. 将十进制转为八进制

 CString str;

 int num = 255;

 str.Format(_T("%o"),num);

 str.Format(_T("%.8o"),num);

 2.TrimRight 和TrimLeft

 函数原型： void CString::TrimLeft/TrimRight(TCHAR chTarget);

 void CString::TrimLetf/TrimRight(LPCTSTR lpszTargets);

 参数：

 chTarget 要被整理的目标字符。

 lpszTargets 指向一个字符串的指针，该字符串包含了要被整理的目标字符串。

 说明：（TrimLeft）

 这个成员函数在没有参数的情况下用来将字符串最前面的空格修整掉。当在没有参数的情况下，TrimLeft删除换行符，空格和Tab键

 这个成员函数的参数用来将一个特点的字符或一群特定的字符从字符串的开始出删除。

 TrimRight () 用于消除从右侧起所遇到的所有空格字符，同时也可用于消除目标字符集合中出现的任意字符，知道遇到第一个不属于目标字符串的字符为止。

 不是清除右边起所出现的完全匹配与目标字符几何的字符串。

 例子：

 CString a = "le.exe";

 a.TrimRight(".exe");

 在执行之后得到的是l，l是第一个不是的，所以留下了。

 

 3.Trim() 用法

 Trim就是两边遍历，也就是分别执行一次TrimLeft()和TrimRight()

 

 4.FindOneOf 用法

 指定多个字符串,然后查找匹配这些字符串其中一个的第一个的位置

 CString strSpec = _T("\\/:*?\"<>!^%|’|&");

 str.findOneof(strSpec);

   
 