---
layout: post
title: OpenCaseCade鼠标移动高亮颜色设置与选中颜色设置
date: 2019-06-26 11:34:07
---

{{ page.title }}
================

<p class="meta">2019-06-26 11:34:07 - 深圳</p>

   OpenCaseCade在鼠标移动至图元时会高亮显示，当图元被选中时默认也会有变化，如何更改高亮的颜色以及选中时的颜色呢

 这个问题以前自己尝试过自己处理改变，但是自己处理实现的效果感觉总是拖拖拉拉 ，而且时效性很差，在最近研究中发现一种方法，可以简单的实现该操作。

 这里要关注一下OCC本身提供的 AIS_InteractiveContext.lxx 文件和AIS_InteractiveObject类

 AIS_InteractiveContext.lxx文件中包含了许多内联函数，这里以前很少关注，但是这次也是从该文件中发现了好多未曾知道的方法。

 实现过程：

 

 一.在OCC环境初始化的时候时候指定高亮显示的颜色与选中的颜色（下面的方法）

 
```
 myAISContext->SetHilightColor(Quantity_NOC_GREEN3);      //高亮颜色
	  
myAISContext->SelectionColor(Quantity_NOC_BLUE3);       //选中颜色
```
 二.在画具体某个图元的时候，设置高亮显示模式（下面代码以画一个基本矩形为例）

 
```
 //画基本矩形
void OccEditView::DrawBaseRect(double Length,double Width,double Height)
{
	RemoveAll();
	m_S1 = BRepPrimAPI_MakeBox(gp_Pnt(0.,0.,0.),Length,Width,Height).Shape();
	m_ais1 = new AIS_Shape(m_S1);

	m_ais1->SetHilightMode(1);   //设置高亮显示模式（目前可以设置为0,1,2）

	myAISContext->SetColor(m_ais1,Quantity_NOC_BLUE3,Standard_False); 
	myAISContext->SetMaterial(m_ais1,Graphic3d_NOM_PLASTIC,Standard_False);    
	myAISContext->Display(m_ais1);

}
```
 此时就完成了，类似的操作细节可以查看OCC库文件，以及参阅开源实现代码。

   
 
