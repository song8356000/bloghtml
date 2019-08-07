---
title: MFC字符串操作（一）MFC CString 成员函数用法大全
date: 2018-09-17 14:50:38
tags: CSDN迁移
---
   CString的构造函数

 CString( );  
 例：CString csStr;  
  
 CString( const CString& stringSrc );  
 例：CString csStr("ABCDEF中文123456");  
 CString csStr2(csStr);  
  
 CString( TCHAR ch, int nRepeat = 1 );  
 例：CString csStr('a',5);  
 //csStr="aaaaa"  
  
 CString( LPCTSTR lpch, int nLength );  
 例：CString csStr("abcdef",3);  
 //csStr="abc"  
  
 CString( LPCWSTR lpsz );  
 例：wchar_t s[]=L"abcdef";  
 CString csStr(s);  
 //csStr=L"abcdef"  
  
 CString( const unsigned char* psz );  
 例：const unsigned char s[]="abcdef";  
 const unsigned char* sp=s;  
 CString csStr(sp);  
 //csStr="abcdef"  
  
 CString( LPCSTR lpsz );  
 例：CString csStr("abcdef");  
 //csStr="abcdef"  
  
 int GetLength( ) const;  
 返回字符串的长度(字符串中的字节计数)，不包含结尾的空字符。  
 例：csStr="ABCDEF中文123456";  
 printf("%d",csStr.GetLength()); //16

 说明:此成员函数用来获取这个CString 对象中的字节计数。这个计数不包括结尾的空字符。 对于多字节字符集（MBCS），GetLength 按每一个8 位字符计数；即，在一个多字节字符中的开始和结尾字节被算作两个字节。 

 void MakeReverse( );  
 颠倒字符串的顺序  
 例：csStr="ABCDEF中文123456";  
 csStr.MakeReverse();  
 cout<<csStr; //654321文中FEDCBA  
  
 void MakeUpper( );  
 将小写字母转换为大写字母  
 例：csStr="abcdef中文123456";  
 csStr.MakeUpper();  
 cout<<csStr; //ABCDEF中文123456  
  
 void MakeLower( );  
 将大写字母转换为小写字母  
 例：csStr="ABCDEF中文123456";  
 csStr.MakeLower();  
 cout<<csStr; //abcdef中文123456  
  
 int Compare( LPCTSTR lpsz ) const;  
 区分大小写比较两个字符串，相等时返回0，大于时返回1，小于时返回-1  
 例：csStr="abcdef中文123456";  
 csStr2="ABCDEF中文123456";  
 cout<<csStr.CompareNoCase(csStr2); //0  
  
 int CompareNoCase( LPCTSTR lpsz ) const;  
 不区分大小写比较两个字符串，相等时返回0，大于时返回1，小于时返回-1  
 例：csStr="abcdef中文123456";  
 csStr2="ABCDEF中文123456";  
 cout<<csStr.CompareNoCase(csStr2); //-1  
  
 int Delete( int nIndex, int nCount = 1 )  
 删除字符，删除从下标nIndex开始的nCount个字符  
 例：csStr="ABCDEF";  
 csStr.Delete(2,3);  
 cout<<csStr; // ABF  
 //当nIndex过大，超出对像所在内存区域时，函数没有任何操作。  
 //当nIndex为负数时，从第一个字符开始删除。  
 //当nCount过大，导致删除字符超出对像所在内存区域时，会发生无法预料的结果。  
 //当nCount为负数时，函数没有任何操作。  
  
 int Insert( int nIndex, TCHAR ch )  
 int Insert( int nIndex, LPCTSTR pstr )  
 在下标为nIndex的位置，插入字符或字符串。返回插入后对象的长度  
 例：csStr="abc";  
 csStr.Insert(2,'x');  
 cout<<csStr; //abxc 

 csStr="abc";  
 csStr.Insert(2,"xyz");  
 cout<<csStr; //abxyzc  
 //当nIndex为负数时，插入在对象开头  
 //当nIndex超出对象末尾时，插入在对象末尾  
  
 int Remove( TCHAR ch );  
 移除对象内的指定字符。返回移除的数目  
 例：csStr="aabbaacc";  
 csStr.Remove('a');  
 cout<<csStr; //bbcc  
  
 int Replace( TCHAR chOld, TCHAR chNew );  
 int Replace( LPCTSTR lpszOld, LPCTSTR lpszNew );  
 替换字串  
 例：csStr="abcdef";  
 csStr.Replace('a','x');  
 cout<<csStr; //xbcdef  
 csStr="abcdef";  
 csStr.Replace("abc","xyz");  
 cout<<csStr; //xyzdef

 

 返回值:返回被替换的字符数。如果这个字符串没有改变则返回零。 

 参数:chOld 要被chNew 替换的字符。 

 chNew 要用来替换chOld 的字符。 

 lpszOld 一个指向字符串的指针，该字符串包含了要被lpszNew 替换的字符。 

 LpszNew 一个指向字符串的指针，该字符串包含了要用来替换lpszOld 的字符。 

 说明:此成员函数用一个字符替换另一个字符。函数的第一个原形在字符串中用chNew 

 现场替换chOld。函数的第二个原形用lpszNew 指定的字符串替换lpszOld 指定的子串。 

 在替换之后，该字符串有可能增长或缩短；那是因为lpszNew 和lpszOld 的长度 

 不需要是相等的。两种版本形式都进行区分大小写的匹配。 

 示例: 

 // 第一个例子，old 和new 具有相同的长度。 

 CString strZap( “C - -” ); 

 int n = strZap.Replace('-', '+' ); 

 ASSERT( n == 2 ); 

 ASSERT(strZap == “C++” ); 

 // 第二个例子，old 和new 具有不同的长度

 CString strBang( “Everybody likes ice hockey” ); 

 n = strBang.Replace( “hockey”, “golf” ); 

 ASSERT( n ==1 ); 

 n = strBang.Replace ( “likes” , “plays” ); 

 ASSERT( n == 1 ); 

 n = strBang.Replace( “ice”, NULL ); 

 ASSERT( n == 1 ); 

 ASSERT( strBang == “Everybody plays golg” ); 

 // 注意，现在在你的句子中有了一个额外的空格。 

 // 要移走这个额外的空格，可以将它包括在要被替换的字符串中，例如，“ice ”。 

 void TrimLeft( );  
 void TrimLeft( TCHAR chTarget );

 说明：如果没有参数,从左删除字符(\n\t空格等),至到遇到一个非此类字符. 当然你也可以指定删除那些字符. 如果指定的参数是字符串,那么遇上其中的一个字符就删除.

 \n 换行符

 \t TAB字符 

 示例1：

 CString str = "\n\t a"; 

 str.TrimLeft(); 

 str为“a”; 

 示例2： 

 CString str = "abbcadbabcadb ";

 str.TrimLeft("ab"); 

 结果"cadbabcadb " 

 str.TrimLeft("ac"); 

 结果"bcadbabcadb " 

 void TrimLeft( LPCTSTR lpszTargets );  
 从左删除字符，被删的字符与chTarget或lpszTargets匹配，一直删到第一个不匹配的字符为止  
 例：csStr="aaabaacdef";  
 csStr.TrimLeft('a');  
 cout<<csStr; //baacdef 

 csStr="aaabaacdef";  
 csStr.TrimLeft("ab");  
 cout<<csStr; //cdef  
 //无参数时删除空格  
  
 void TrimRight( );  
 void TrimRight( TCHAR chTarget );  
 void TrimRight( LPCTSTR lpszTargets );  
 从右删除字符，被删的字符与chTarget或lpszTargets匹配，一直删到第一个不匹配的字符为止  
 例：csStr="abcdeaafaaa";  
 csStr.TrimRight('a');  
 cout<<csStr; //abcdeaaf  
 csStr="abcdeaafaaa";  
 csStr.TrimRight("fa");  
 cout<<csStr; //abcde  
 //无参数时删除空格  
  
 void Empty( );  
 清空  
 例：csStr="abcdef";  
 csStr.Empty();  
 printf("%d",csStr.GetLength()); //0  
  
 BOOL IsEmpty( ) const;  
 测试对象是否为空，为空时返回零，不为空时返回非零  
 例：csStr="abc";  
 cout<<csStr.IsEmpty(); //0; 

 csStr.Empty();  
 cout<<csStr.IsEmpty(); //1;  
  
 int Find( TCHAR ch ) const;  
 int Find( LPCTSTR lpszSub ) const;  
 int Find( TCHAR ch, int nStart ) const;  
 int Find( LPCTSTR pstr, int nStart ) const;  
 查找字串，nStart为开始查找的位置。未找到匹配时返回-1，否则返回字串的开始位置  
 例：csStr="abcdef";  
 cout<<csStr.Find('b'); //1 

 cout<<csStr.Find("de"); //3  
 cout<<csStr.Find('b',3); //-1  
 cout<<csStr.Find('b',0); //1  
 cout<<csStr.Find("de",4); //-1  
 cout<<csStr.Find("de",0); //3  
 //当nStart超出对象末尾时，返回-1  
 //当nStart为负数时，返回-1  
  
 int FindOneOf( LPCTSTR lpszCharSet ) const;  
 查找lpszCharSet中任意一个字符在CString对象中的匹配位置。未找到时返回-1，否则返回字串的开始位置  
 例：csStr="abcdef";  
 cout<<csStr.FindOneOf("cxy"); //2  
  
 CString SpanExcluding( LPCTSTR lpszCharSet ) const;  
 返回对象中与lpszCharSet中任意匹配的第一个字符之前的子串  
 例：csStr="abcdef";  
 cout<<csStr.SpanExcluding("cf"); //ab  
  
 CString SpanIncluding( LPCTSTR lpszCharSet ) const;  
 从对象中查找与lpszCharSe中任意字符不匹配的字符，并返回第一个不匹配字符之前的字串  
 例：csStr="abcdef";  
 cout<<csStr.SpanIncluding("fdcba"); //abcd  
  
 int ReverseFind( TCHAR ch ) const;  
 从后向前查找第一个匹配，找到时返回下标。没找到时返回-1  
 例：csStr="abba";  
 cout<<csStr.ReverseFind('a'); //3  
  
 void Format( LPCTSTR lpszFormat, ... );  
 void Format( UINT nFormatID, ... );  
 格式化对象，与C语言的sprintf函数用法相同  
 例：csStr.Format("%d",13);  
 cout<<csStr; //13  
  
 TCHAR GetAt( int nIndex ) const;  
 返回下标为nIndex的字符，与字符串的[]用法相同  
 例：csStr="abcdef";  
 cout<<csStr.GetAt(2); //c  
 //当nIndex为负数或超出对象末尾时，会发生无法预料的结果。  
  
 void SetAt( int nIndex, TCHAR ch );  
 给下标为nIndex的字符重新赋值  
 例：csStr="abcdef";  
 csStr.SetAt(2,'x');  
 cout<<csStr; //abxdef  
 //当nIndex为负数或超出对象末尾时，会发生无法预料的结果。  
  
 CString Left( int nCount ) const;  
 从左取字串  
 例：csStr="abcdef";  
 cout<<csStr.Left(3); //abc  
 //当nCount等于0时，返回空。  
 //当nCount为负数时，返回空。  
 //当nCount大于对象长度时，返回值与对象相同。  
  
 CString Right( int nCount ) const;  
 从右取字串  
 例：csStr="abcdef";  
 cout<<csStr.Right(3); //def  
 //当nCount等于0时，返回空  
 //当nCount为负数时，返回空  
 //当nCount大于对象长度时，返回值与对象相同  
  
 CString Mid( int nFirst ) const;  
 CString Mid( int nFirst, int nCount ) const;  
 从中间开始取字串  
 例：csStr="abcdef";  
 cout<<csStr.Mid(2); //cdef 

 csStr="abcdef";  
 cout<<csStr.Mid(2,3); //cde  
 //当nFirst为0和为负数时，从第一个字符开始取  
 //当nFirst等于对象末尾时，返回空字串  
 //当nFirst超出对象末尾时，会发生无法预料的结果  
 //当nCount超出对象末尾时，返回从nFirst开始一直到对象末尾的字串  
 //当nCount为0和为负数时，返回空字串  
  
 LPTSTR GetBuffer( int nMinBufLength );  
 申请新的空间，并返回指针  
 例：csStr="abcde";  
 LPTSTR pStr=csStr.GetBuffer(10);  
 strcpy(pStr,"12345");  
 csStr.ReleaseBuffer();  
 pStr=NULL;  
 cout<<csStr //12345  
 //使用完GetBuffer后，必须使用ReleaseBuffer以更新对象内部数据，否则会发生无法预料的结果

 

 返回值：一个指向对象的（以空字符结尾的）字符缓冲区的LPTSTR 指针。 

 参数：nMinBufLength 

 字符缓冲区的以字符数表示的最小容量。这个值不包括一个结尾的空字符的空间。 

 说明：此成员函数返回一个指向CString 对象的内部字符缓冲区的指针。返回的LPTSTR 不是const，因此可以允许直接修改CString 的内容。如果你使用由GetBuffer 返回的指针来改变字符串的内容，你必须在使用其它的CString 成员函数之前调用ReleaseBuffer 函数。

 在调用ReleaseBuffer 之后，由GetBuffer 返回的地址也许就无效了，因为其它的CString 操作可能会导致CString 缓冲区被重新分配。如果你没有改变此CString 的长度，则缓冲区不会被重新分配。当此CString 对象被销毁时，其缓冲区内存将被自动释放。

 注意：如果你自己知道字符串的长度，则你不应该添加结尾的空字符。但是，当你用ReleaseBuffer 来释放该缓冲区时，你必须指定最后的字符串长度。如果你添加了结尾的空字符，你应该给ReleaseBuffer 的长度参数传递-1 ，ReleaseBuffer 将对该缓冲区执行strlen 来确定它的长度。 

 示例： 

 // CString::GetBuffer 例子 

 CString s( "abcd" ); 

 #ifdef _DEBUG 

 afxDump << "CString s " << s << "\n"; 

 #endif 

 LPTSTR p = s.GetBuffer( 10 ); 

 strcpy( p, "Hello" ); // 直接访问CString 对象

 s.ReleaseBuffer( ); 

 #ifdef _DEBUG 

 afxDump << "CString s " << s << "\n"; 

 #endif 

 void ReleaseBuffer( int nNewLength = -1 );  
 使用GetBuffer后，必须使用ReleaseBuffer以更新对象内部数据  
 例：csStr="abc";  
 LPTSTR pStr=csStr.GetBuffer(10);  
 strcpy(pStr,"12345");  
 cout<<csStr.GetLength(); //3(错误的用法)  
 csStr.ReleaseBuffer();  
 cout<<csStr.GetLength(); //5(正确) 

 pStr=NULL;  
 //CString对象的任何方法都应在ReleaseBuffer之后调用

 //补充

 参数:nNewLength

 此字符串的以字符数表示的新长度，不计算结尾的空字符。如果这个字

 符串是以空字符结尾的，则参数的缺省值－1 将把CString 的大小设置为

 字符串的当前长度。

 说明：

 使用ReleaseBuffer 来结束对由GetBuffer 分配的缓冲区的使用。如果你知道缓

 冲区中的字符串是以空字符结尾的，则可以省略nNewLength 参数。如果字符

 串不是以空字符结尾的，则可以使用nNewLength 指定字符串的长度。在调用

 ReleaseBuffer 或其它CString 操作之后，由GetBuffer 返回的地址是无效的。

 示例：

 下面的例子说明了如何使用CString::ReleaseBuffer。

 // CString::ReleaseBuffer 示例

 CString s;

 s = "abc";

 LPTSTR p = s.GetBuffer( 1024 );

 strcpy(p, "abc"); // 直接使用该缓冲区

 ASSERT( s.GetLength() == 3 ); // 字符串长度 = 3

 s.ReleaseBuffer(); // 释放多余的内存，现在p 无效。

 ASSERT( s.GetLength() == 3 ); // 长度仍然是3  
  
 LPTSTR GetBufferSetLength( int nNewLength );  
 申请新的空间，并返回指针  
 例：csStr="abc";  
 csStr.GetBufferSetLength(20);  
 cout<<csStr; //abc 

 count<<csStr.GetLength(); //20; 

 csStr.ReleaseBuffer();  
 count<<csStr.GetLength(); //3;  
 //使用GetBufferSetLength后可以不必使用ReleaseBuffer

 来源：[https://blog.csdn.net/shufac/article/details/20737799](https://blog.csdn.net/shufac/article/details/20737799)

   
 