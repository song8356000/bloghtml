---
layout: post
title: OpenCaseCade在一个窗体中的两个picture控件中分别显示
date: 2019-06-24 14:40:01
---

{{ page.title }}
================

<p class="meta">2019-06-24 14:40:01 - 深圳</p>

   经常使用的方式是直接在一个窗口或者一个picture控件中显示OCC的绘制图元，如果要想在一个窗口中显示多个picture控件，并且每个绘制控件互不影响，该如何实现。

 通过简单地测试，结合OCC官方参考资料，得出以下方法：

 **在OCC环境初始化的时候 V3d_Viewer 类的对象初始化个数可以初始化多个，每个绑定一个picture控件即可，至于其它鼠标操作，注意区分各个显示控件位置即可区分。**

 下面是我实现的一个窗口有两个picture控件的实现方式，多个同理。

 
```
 void OccEditView::Init(HWND hwnd,HWND hwnd2)   //参数为显示控件句柄
{
	try
	{
		Handle(Aspect_DisplayConnection) aDisplayConnection;
		myGraphicDriver = new OpenGl_GraphicDriver (aDisplayConnection);
	}
	catch(Standard_Failure)
	{
		ExitProcess (1);
	}

	TCollection_ExtendedString a3DName("Visu3D");
	myViewer = new V3d_Viewer(myGraphicDriver,a3DName.ToExtString()); 

	myViewer->SetDefaultLights();
	myViewer->SetLightOn();
	myViewer->SetDefaultBackgroundColor(Quantity_NOC_GRAY44);//改变背景颜色
	

	myAISContext =new AIS_InteractiveContext(myViewer);

	myAISContext->SetDisplayMode(AIS_Shaded,Standard_False);

	
	myView = myViewer->CreateView();

	myView->SetComputedMode(Standard_False);

	Handle(WNT_Window) aWNTWindow = new WNT_Window(hwnd);

	myView->SetWindow(aWNTWindow); 

	
	if (!aWNTWindow->IsMapped()) 
	{
		aWNTWindow->Map(); 
	}




	//第二个显示控件

	TCollection_ExtendedString a3DName2("Visu3D");
	myViewer2 = new V3d_Viewer(myGraphicDriver,a3DName2.ToExtString()); 

	myViewer2->SetDefaultLights();
	myViewer2->SetLightOn();
	myViewer2->SetDefaultBackgroundColor(Quantity_NOC_GRAY44);//改变背景颜色


	myAISContext2 =new AIS_InteractiveContext(myViewer2);

	myAISContext2->SetDisplayMode(AIS_Shaded,Standard_False);



	myView2 = myViewer2->CreateView();

	myView2->SetComputedMode(Standard_False);

	Handle(WNT_Window) aWNTWindow2 = new WNT_Window(hwnd2);

	myView2->SetWindow(aWNTWindow2); 


	if (!aWNTWindow2->IsMapped()) 
	{
		aWNTWindow2->Map(); 
	}


}
```
 上述代码只涉及OCC环境初始化部分，在具体实现其它细节的时候可根据自己的需求添加

   
 
