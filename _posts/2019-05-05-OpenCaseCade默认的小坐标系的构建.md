---
layout: post
title: OpenCaseCade默认的小坐标系的构建
date: 2019-05-05 11:21:06
---

{{ page.title }}
================

<p class="meta">2019-05-05 11:21:06 - 深圳</p>

 版权声明：需要转载的请注明出处 
   我所说的这个小坐标系其实就是下图，会根据当前视图旋转的小参考坐标系：

 ![](https://img-blog.csdnimg.cn/20190505104455284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9zdHVkeS1saWZlLmJsb2cuY3Nkbi5uZXQ=,size_16,color_FFFFFF,t_70)

 这个小的坐标系在之前使用的时候，是自己绘制的，自己画的，不但不好看，而且实用性也很差，在后来偶然间发现这个小坐标系模型OCC是自己提供接口的，只需要在初始化环境之后，设置相关参数就可以了。

 相关方法如下：

 
```
 myView = myViewer->CreateView();
	
	myView->SetComputedMode(Standard_False);

	Handle(WNT_Window) aWNTWindow = new WNT_Window(this->GetSafeHwnd());  
	myView->SetWindow(aWNTWindow); 
    
	if (!aWNTWindow->IsMapped()) 
	{
		aWNTWindow->Map(); 
	}

	aWNTWindow->SetBackground(Quantity_NOC_YELLOW);

	myView->TriedronDisplay(Aspect_TOTP_LEFT_LOWER, Quantity_NOC_WHITE, 0.06, V3d_ZBUFFER);   //画三维坐标系
```
 myView的TriedronDisplay方法提供了这个功能，在OCC6.8.0中关于这个小坐标轴的使用方法有三个相关接口：

 **1.创建小坐标系模型**

 
```
 Standard_EXPORT   void TriedronDisplay (const Aspect_TypeOfTriedronPosition APosition = Aspect_TOTP_CENTER, const Quantity_NameOfColor AColor = Quantity_NOC_WHITE, const Standard_Real AScale = 0.02, const V3d_TypeOfVisualization AMode = V3d_WIREFRAME) ;
```
 分别需要指定显示的位置，X,Y,Z字母的颜色，以及显示的样式三个参数，数据类型是OCC内部数据类型。

 ** 2.删除小坐标系模型**  
 Standard_EXPORT void TriedronErase() ;

 ** 3.** **Highlights the echo zone of the Triedron.**

 此功能目前还没用到  
 Standard_EXPORT void TriedronEcho (const Aspect_TypeOfTriedronEcho AType = Aspect_TOTE_NONE) ;

 

 上面只是介绍了最简单的使用方式，相关实现细节，OpenCaseCade源代码中有相关实现，开源的，可以自行参考

   
 
