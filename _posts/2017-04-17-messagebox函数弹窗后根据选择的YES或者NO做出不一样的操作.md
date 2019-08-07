---
title: messagebox函数弹窗后根据选择的YES或者NO做出不一样的操作
date: 2018-06-06 15:59:00
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/80596320   
   messagebox一类函数在C，C++中很常见，MFC中更多，此函数弹出一个对话框之后，根据书写代码的不同属性（MB_OK 一类）会让用户选择不同的按钮，如果再点击某个按钮是想要进行不同的操作，该如何做呢？

这里贴上我写的一段代码：

根据messagebox弹窗后选择YES NO CANCLE做出不同的一些操作


```
int result;
	CString showstr;
	HMODULE hmodule = LoadLibrary(_T("MFCdll_commom.dll"));
	if (hmodule!=NULL)
	{
		one testfun = (one)GetProcAddress(hmodule, "test");
		if (testfun!=NULL)
		{
			result = testfun(5, 5);
			showstr.Format(_T("%d"), result);
			int one;
			one = AfxMessageBox(_T("测试结果会显示在右边的框里面"), MB_YESNOCANCEL);
			switch (one)
			{
			case 6:
				SetDlgItemText(IDC_EDIT1, showstr);
				break;
			case 7:
				MessageBox(_T("IDNO"));
				break;
			case 2:
				MessageBox(_T("IDCANCLE"));
				break;
			default:
				break;
			}
```
  
   
 