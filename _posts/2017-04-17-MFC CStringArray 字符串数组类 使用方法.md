---
title: MFC CStringArray 字符串数组类 使用方法
date: 2018-10-30 16:00:57
tags: CSDN迁移
---
   ```
 void CCStringArrayTestDlg::OnBnClickedButton1()
{
	// TODO: 在此添加控件通知处理程序代码
	CStringArray as;
	as.Add("11");//加数据
	as.Add("22");
	as.Add("33");
	as.Add("44");
	int   size=as.GetSize();   //得长度
	for(int   i=0;i < size; i++)
	{   //的数据
		CString strTmp=as.GetAt(i);   
	}   
	as.InsertAt(2,"00");
	  size=as.GetSize();   //得长度
	for(int   i=0;i < size; i++)
	{   //的数据
		CString strTmp=as.GetAt(i);   
	}   
	as.RemoveAt(2);
	  size=as.GetSize();   //得长度
	for(int   i=0;i < size; i++)
	{   //的数据
		CString strTmp=as.GetAt(i);   
	}   
	as.RemoveAll();
}
void CCStringArrayTestDlg::OnBnClickedButton2()
{
	// TODO: 在此添加控件通知处理程序代码
	CStringArray   as;  
	as.Add("aaaa");  
	as.Add("bbb");  
	as.Add("CCC");  
	as.Add("dddddddddddddd");  
	int   size=as.GetSize();  
	CStdioFile   file;  
	CString   strTemp;  
	file.Open("Save.txt",CFile::modeCreate|CFile::modeWrite);  
	for(int   i=0;i < size; i++)
	{  
		strTemp=as.GetAt(i);  
		file.WriteString(strTemp+"\n");  
	}  
	file.Close();
}
void CCStringArrayTestDlg::OnBnClickedButton3()
{
	// TODO: 在此添加控件通知处理程序代码
	//CArray arrroads;
	CStringArray road;
	CString temp="a|b|c|d|e";
	int s0=temp.ReverseFind('|');
	road.Add(temp.Mid(s0+1));
	while(s0>0)
	{
		temp=temp.Mid(0,s0);
		s0=temp.ReverseFind('|');
		road.Add(temp.Mid(s0+1));
	}
 
	int n=road.GetSize();
	for(int i=0;i<n;i++)
	{
		MessageBox(road.GetAt(i));
	}
}

```
 CStringArray--字符串数组类  
 CStringArray类支持CString对象数组。  
 在使用一个数组之前，使用SetSize来建立它的大小并给它分配内存。如果你不使用SetSize，则向数组中添加元素时将导致数组被频繁地拷贝和分配内存。频繁分配内存和拷贝会导致效率低和内存零碎。  
 如果你需要数组中个别字符串元素的转储，则应该将转储环境的深度设置为1或更大。当一个CString数组被删除时，或当其中的个别元素被删除时，字符串内存被根据需要释放。  
 CStringArray类成员  
 构造  
 CStringArray构造一个空的CString对象数组  
 绑定  
 GetSize 获取这个数组中的元素数目  
 SetSize 设置这个数组中包含的元素数目  
 GetUpperBound 返回最大的有效索引  
 操作符  
 FreeExtra 释放当前数组边界之外的未使用的所有内存  
 RemoveAll 从数组中删除所有元素  
 元素访问  
 GetAt 返回位于给定索引处的值  
 SetAt 设置给定索引处的元素的值；不得将数组增大  
 ElementAt 返回对数组中的某一元素指针的临时引用  
 GetData 对数组中的元素允许的访问。可以是NULL  
 扩大数组  
 SetAtGrow 设置给定索引处的值，如果必要的话可以增长数组  
 Add 在数组的末尾添加一个元素；可根据需要增长数组  
 Append 向数组中添加另一个数组；如果必要的话可增长数组  
 Copy 将另一个数组拷贝到此数组中；如果必要的话可增长数组  
 插入/删除  
 InsertAt 在指定索引处插入一个元素（或者是另一个数组中的所有元素）  
 RemoveAt 删除指定索引处的一个元素  
 操作符  
 operator [] 设置或获取在指定索引处的元素

   
 