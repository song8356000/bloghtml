---
title: MFC字符串操作（三）MFC CString其他用法小结
date: 2018-09-17 14:54:32
tags: CSDN迁移
---
   1. 初始化方法：

 (1) 直接复制，如CString=”mingrisoft”;

 (2) 通过构造函数初始化，如 CString str(‘ ’,100)//与分配100个字节，填充空格

 char* p=”feiqiang”; Cstring(p);delete p.

 (3) 加载工程中的字符串资源，如CString str;str.LoadString(IDS_STR);

 (4) 使用CString类的成员函数Format初始化，如CString str; int i=0; str.Format(“value:%d”,i);

 

 2. CString与其他类型之间的转换

 1) 将CString转化为char*，如

 CString str=”feqiang”;

 char *p;

 p=str.GetBuffer();

 delete p;

 

 2) 将char*转化为CString，如：

 char* p=”feiqiang”;

 p[len(p)]=’\0’;

 Cstring str(p);

 char* 和char数组的转化：

 char buf[5] ={‘a’,’b’,’c’};

 char *p;

 p=new char[5];

 p=buf;

 

 3) 将字符串转化为数字：

 CString str=”12”;

 int i=atoi(str);

 long j=atoll(str);

 float f=atof(str);

 

 4) 将数字转化为字符串：

 CString str;

 int i=12;

 str.Format(“%d”,i);

 long j=12;

 str,Format(“%ld”,j);

 同理其他内置类型。

 

 3. 字符串的相关操作即方法的使用：

 (1) 提取字符串中的中文，如

 CString strtext,temp,strres;

 GetLlgItem(IDC_TEXT)->GetWindowText(strtext);//通过ID获取编辑框中的文本

 for(int =0;i<strtext.GetLength();i++){

 char ch=strtext.GetAt(i); 

 if(IsDBCSLeadByte(ch)){ //判断字符是否是双字节编码的前一个字节

 tmp=strtext.Mid(i,2);//截取索引index1到index2的字符[ )

 i++;

 stress+=tmp;

 }

 GetLlgItem(IDC_RESULT)->SetWindowText(strtes);//设置文本框中的文本

 }

 (2) 英文字符串首字母大写，如 将以空格符分隔的英文单词的第一个字母大写(小写)

 str.GetAt(i);//提取字符串索引为i个字母

 str.MakeLower();//转化为小写

 tmp.MakeUpper();//转化为大写

 (3) 按制定符号分割字符：

 int pos=str.Find(strchar);//查找字符，如果没找到，则返回0，找到则返回字符的位置,参数可以是字符也可以是字符串

 while(pos>0){

 str1=str.Left(pos);//取左,参数为字符串的个数

 str=str.Right(str.GetLength-pos-1);//取右，参数同上

 tmp.Format(“%s\r\n”,str1);//字符串中\r\n代表回车化行符

 strres+=tmp;

 pos=str.Find(strchar);

 }

 (4) 删除指定的中文:

 m_text.GetWindowText(strtxt);

 m_text.GetSel(istart,iend);//得到文本框中选中的文本，并且得到文本的头索引和尾索引

 if(istart==iend){

 return;

 }

 str1=strtxt.Left(istart);

 if(iend>=strtxt.GetLength()){

 str2=””;

 }else{

 str2=strtxt.Right(strtxt.GetLength()-iend);

 }

 strres+=str1;

 strres+=str2;

 (5) 替换字符串：

 strtxt.Replace(strchar,strnew);//用新串替换旧串

 (6) 根据CPoint查找位置：

 CPoint pt;//获取字符串时获取鼠标所在字符串的位置

 int pos=m_text.CharFromPos(pt);//根据pt获取字符串中的位置，即其左侧字符的位置

 if(str.IsEmpty()){//判断字符串是否为空

 m_num.AddString(strres);//文本框追加字符串

 }

 将字符转化为大写：ch=ch-32；

 (7) 字符串忽略大小写的比较：

 CString str=”feiqiang”;

 int com=str.CompareNoCase(“mingri”);//如果相等返回0，否则返回-1；

 (8)连接换行符：CString str=”feiqiang\t”;

 (9)字符反转：str.MakeReverse();

 (10) 取出首位空格：str.TrimLeft(); str.TrimRight();

 取出字符串中的所有空格，str.Replace(“ ”,””);

 (11) 在ListBox中查找字符串

 int index=::SendMessage(m_stringlist.GetSafeHwnd(),LB_FINDSTRINGEXACT,-1,  
 (LPARAM)(LPCTSTR)strtext));//通过SendMessage函数向列表控件发送LB_FINDSTRINGEXACT消息来查找指定字符串是否在列表空间中，如果存在则返回索引位置。

 (12) 字符串数组：

 CString str[5] array;

 CString str[5]={“feiqiang”,”mingri”,”mr”};

 for(int i=0;i<5;i++){

 array.Add(str[i]);//添加元素

 }

 for(int j=0;j<array.Size(),j++){//字符数组大小

 if(array.Get(j)==”mr”){

 MessageBox(“存在”);

 }

 }

 (13) 设置编码方式:Project/SettingsàPreprocessor，如果要使用DBCS，则添加_MBCS(多个字节编码)，如果要使用Unicode，则添加_Unicode,不添加则使用ASCII.

 

 二 字符串指针类型

 (1) LPCSTR:32位静态字符串指针，可以直接赋值使用，如LPCSTR str=”mingrisofg”;

 (2) LPSTR:32位字符串指针，如LPSTR str; str=new char[256];

 (3) LPCTSTR:32位UNICODE型静态字符串指针，如 LPCTSTR str=_T(“mingrisoft”);

 (4) LPTSTR: 32位UNICODE型字符串指针，如LPTSTR str=new TCHAR[256];

 

 三 BSTR(进行COM编程时使用的字符串类型)与CString之间的转化：

 1. 对BSTR变量赋值时：

 BSTR bstr=NULL;

 bstr=SysAllocString(L”feiqang”);//从LPCWSTR构造

 SysFreeString(bstr);//释放

 将BSTR强制转化为CString，如：

 CString str=(CString) bstr;或CString str; BSTR bstr=str.AllocSysString();

 2. _bstr_(对BSTR的包装类)，包含的头文件为：”COMDEF.H”

 用法：

 直接赋值：_bstr_t tbstr=”feqiang”;

 给CString对象赋值：CString str=(LPCSTR)tbstr;//LPCSTR str=tbstr;

 将_bsr_转化为BSTR，使用copy函数：BSTR bstr=tbstr.copy(); SysFreeString(bstr);

 BSTR之间赋值给_bstr_对象，如BSTR bstr=SysAllocString(L”mingri”); _bstr_t tbstr=bstr;

   
 