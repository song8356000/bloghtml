---
layout: post
title: OpenCaseCade拓扑几何的布尔运算
date: 2019-07-25 17:14:46
---

{{ page.title }}
================

<p class="meta">2019-07-25 17:14:46 - 深圳</p>

   # **一.首先简单介绍一下布尔运算：**

 布尔运算是数字符号化的逻辑推演法，包括**联合、相交、相减**。在图形处理操作中引用了这种逻辑运算方法以使简单的基本图形组合产生新的形体，并由二维布尔运算发展到三维图形的布尔运算。

 Boolean（布尔运算）通过对两个以上的物体进行并集、差集、交集的运算，从而得到新的物体形态。系统提供了4种布尔运算方式：**Union（并集）、Intersection（交集）和Subtraction（差集**，包括A-B和B-A两种）。

 
# **二.OCC中拓扑几何布尔运算的实现**

 结合OCC提供的基本例子程序，以及自己的亲身实践与资料的查阅，OCC提供的拓扑几何布尔运算的方法在以下几个类中（我使用的是OpenCaseCade6.8.0，其它版本有可能有稍微差距）：

 

 
## **1.BRepAlgoAPI_Section OCC布尔运算（求两个图形相交在一起的相交线）**

 **功能：**计算两个拓扑SHAPE或者两个几何的相交（交集），可以理解为，由S2切割S1，返回在S1上所切割轨迹。

 ** OCC类构造函数：**

 
```
 //两个拓扑SHAPE相交
 Standard_EXPORT BRepAlgoAPI_Section(const TopoDS_Shape& S1, const TopoDS_Shape& S2, const BOPAlgo_PaveFiller& aDSF, const Standard_Boolean PerformNow = Standard_True);

//两个拓扑SHAPE相交
  Standard_EXPORT BRepAlgoAPI_Section(const TopoDS_Shape& Sh1, const TopoDS_Shape& Sh2, const Standard_Boolean PerformNow = Standard_True);
  
  //拓扑SHAPE与几何平面相交
  Standard_EXPORT BRepAlgoAPI_Section(const TopoDS_Shape& Sh, const gp_Pln& Pl, const Standard_Boolean PerformNow = Standard_True);
  
  //拓扑SHAPE与几何曲面相交
  Standard_EXPORT BRepAlgoAPI_Section(const TopoDS_Shape& Sh, const Handle(Geom_Surface)& Sf, const Standard_Boolean PerformNow = Standard_True);
  
  //几何曲面与拓扑SHAPE相交
  Standard_EXPORT BRepAlgoAPI_Section(const Handle(Geom_Surface)& Sf, const TopoDS_Shape& Sh, const Standard_Boolean PerformNow = Standard_True);

//两个几何曲面相交
Standard_EXPORT BRepAlgoAPI_Section(const Handle(Geom_Surface)& Sf1, const Handle(Geom_Surface)& Sf2, const Standard_Boolean PerformNow = Standard_True);
```
 ** 使用实例：（OCC示例程序中参考），该代码是显示的一个球体切割基本管形所产生的切割轨迹，经过运算形成的是一个，包含切割轨迹的基本管形，每条切割线，是一个单独的图元。上面构造函数中还有许多种不同形状之间的运算，基本包含了每种几何图元的运算。**

 
```
 //基本圆管
TopoDS_Shape atorus = BRepPrimAPI_MakeTorus(120, 20).Shape();
Handle(AIS_Shape) ashape=new AIS_Shape(atorus);
myAISContext->SetColor(ashape,Quantity_NOC_RED,Standard_False);
myAISContext->SetMaterial(ashape,Graphic3d_NOM_PLASTIC,Standard_False);    
myAISContext->SetDisplayMode(ashape,1,Standard_False);
myAISContext->SetTransparency(ashape,0.1,Standard_False);
myAISContext->Display(ashape,Standard_False);

gp_Vec V1(1,1,1);
Standard_Real radius = 120;
Standard_Integer i=-3;


//基本球体穿过基本圆管的切割轨迹
for(i;i<=1;i++) {

    //基本球体
    TopoDS_Shape asphere = BRepPrimAPI_MakeSphere(gp_Pnt(26 * 3 * i, 0, 0), radius).Shape();

    Handle (AIS_Shape) theShape=new AIS_Shape (asphere);
    myAISContext->SetTransparency(theShape,0.1,Standard_False);
    myAISContext->SetColor(theShape,Quantity_NOC_WHITE,Standard_False);
    myAISContext->SetDisplayMode(theShape,1,Standard_False);
    myAISContext->Display(theShape,Standard_False);
	Fit();
    Standard_Boolean PerformNow=Standard_False; 

    // 进行布尔运算
    BRepAlgoAPI_Section section(atorus,asphere,PerformNow);
    section.ComputePCurveOn1(Standard_True);
    section.Approximation(TopOpeBRepTool_APPROX);
    section.Build();

    Handle(AIS_Shape) asection=new AIS_Shape(section.Shape());
    myAISContext->SetDisplayMode(asection,0);
    myAISContext->SetColor(asection,Quantity_NOC_WHITE);
    myAISContext->Display(asection);
    if(i<1) {
    myAISContext->Remove(theShape);
	}
}
```
 
## **2.BRepAlgoAPI_Common OCC布尔运算（可求两个形状相交部分构成的的新形状）**

 **功能：**布尔Intersection运算，运算结果为两个图形相交部分，可以构成新的形状。

 **构造函数：**

 
```
  //! Constructs a common part for shapes aS1 and aS2 .
  Standard_EXPORT BRepAlgoAPI_Common(const TopoDS_Shape& S1, const TopoDS_Shape& S2);
  
  Standard_EXPORT BRepAlgoAPI_Common(const TopoDS_Shape& S1, const TopoDS_Shape& S2, const BOPAlgo_PaveFiller& aDSF);
```
 **使用实例：该代码是将一个基本句型与一个基本楔型进行**布尔Intersection运算，显示运算后生成的新的形状

 
```
 //基本矩形
gp_Ax2 axe(gp_Pnt(10,10,10),gp_Dir(1,2,1));
TopoDS_Shape theBox = BRepPrimAPI_MakeBox(axe, 60, 80, 100).Shape();

Handle(AIS_Shape) aboxshape=new AIS_Shape(theBox);
myAISContext->SetColor(aboxshape,Quantity_NOC_YELLOW,Standard_False);
myAISContext->SetMaterial(aboxshape,Graphic3d_NOM_PLASTIC,Standard_False);    
myAISContext->SetDisplayMode(aboxshape,1,Standard_False);
myAISContext->SetTransparency(aboxshape,0.2,Standard_False);
myAISContext->Display(aboxshape,Standard_False);
myAISContext->SetCurrentObject(aboxshape,Standard_False);
Fit();
Sleep(500);

//基本楔型
TopoDS_Shape theWedge = BRepPrimAPI_MakeWedge(60., 100., 80., 20.).Shape();
Handle(AIS_Shape) awedge = new AIS_Shape(theWedge);
myAISContext->SetColor(awedge,Quantity_NOC_RED,Standard_False);
myAISContext->SetMaterial(awedge,Graphic3d_NOM_PLASTIC,Standard_False);    
myAISContext->SetTransparency(awedge,0.0,Standard_False);
myAISContext->Display(awedge,Standard_False);
myAISContext->SetCurrentObject(awedge,Standard_False);
Fit();
Sleep(500);

//基本矩形与基本楔型进行布尔交（Intersection）运算
TopoDS_Shape theCommonSurface = BRepAlgoAPI_Common(theBox,theWedge);

//删除基本矩形与基本楔型
myAISContext->Erase(aboxshape,Standard_True);
myAISContext->Erase(awedge,Standard_True);

//显示基本矩形与基本楔型进行布尔交（Intersection）运算产生的新图形
Handle(AIS_Shape) acommon = new AIS_Shape(theCommonSurface);
myAISContext->SetColor(acommon,Quantity_NOC_GREEN,Standard_False); 
myAISContext->SetMaterial(acommon,Graphic3d_NOM_PLASTIC,Standard_False);    
myAISContext->Display(acommon,Standard_False);
myAISContext->SetCurrentObject(acommon);
```
 
## **3.BRepAlgoAPI_Cut OCC布尔运算（减法,可以从一个形状中减去另一种形状）**

 **功能：**布尔差运算，扣除相交部分，并去除CUT SHAPE

 **构造函数：**

 
```
   //! Shape aS2 cuts shape aS1. The
  //! resulting shape is a new shape produced by the cut operation.
  Standard_EXPORT BRepAlgoAPI_Cut(const TopoDS_Shape& S1, const TopoDS_Shape& S2);
  
  //! Constructs a new shape cut from
  //! shape aS1 by shape aS2 using aDSFiller (see
  //! BRepAlgoAPI_BooleanOperation Constructor).
  Standard_EXPORT BRepAlgoAPI_Cut(const TopoDS_Shape& S1, const TopoDS_Shape& S2, const BOPAlgo_PaveFiller& aDSF, const Standard_Boolean bFWD = Standard_True);
```
 **使用实例：该代码是将一个基本矩形与一个基本球体进行布尔差运算，去除与球体与矩形相交的部分，形成新的形状，只显示新的形状。**

 
```
 //基本矩形
TopoDS_Shape theBox = BRepPrimAPI_MakeBox(200, 60, 60).Shape();

Handle (AIS_Shape)	ais1 = new AIS_Shape(theBox);
myAISContext->SetDisplayMode(ais1,1,Standard_False);
myAISContext->SetColor(ais1,Quantity_NOC_GREEN,Standard_False);
myAISContext->SetMaterial(ais1,Graphic3d_NOM_PLASTIC,Standard_False);
myAISContext->Display(ais1,Standard_False);
myAISContext->SetCurrentObject(ais1,Standard_False);
Fit();
Sleep(1000);

//基本球体
TopoDS_Shape theSphere = BRepPrimAPI_MakeSphere(gp_Pnt(100, 20, 20), 80).Shape();
Handle (AIS_Shape)	ais2 = new AIS_Shape(theSphere);
myAISContext->SetDisplayMode(ais2,1,Standard_False);
myAISContext->SetColor(ais2,Quantity_NOC_YELLOW,Standard_False);
myAISContext->SetMaterial(ais2,Graphic3d_NOM_PLASTIC,Standard_False);
myAISContext->Display(ais2,Standard_False);
myAISContext->SetCurrentObject(ais2,Standard_False);
Fit();
Sleep(1000);

//基本矩形与基本球体进行几何差运算，形成新的形状
TopoDS_Shape ShapeCut = BRepAlgoAPI_Cut(theSphere,theBox);

//隐藏基本矩形与基本球体的显示
myAISContext->Erase(ais1,Standard_False);
myAISContext->Erase(ais2,Standard_False);

//显示通过几何差运算生成的新的形状
Handle (AIS_Shape)	aSection = new AIS_Shape(ShapeCut);
myAISContext->SetDisplayMode(aSection,1,Standard_False);
myAISContext->SetColor(aSection,Quantity_NOC_RED,Standard_False);
myAISContext->SetMaterial(aSection,Graphic3d_NOM_PLASTIC,Standard_False);
myAISContext->Display(aSection,Standard_False);
myAISContext->SetCurrentObject(aSection,Standard_False);
Fit();
```
 
## **4.BRepAlgoAPI_Fuse OCC布尔运算（加法,将两个图形结合成一个图形）**

 **功能：**布尔Union运算，运算结果，是两个图形合并，形成新的形状

 **构造函数：**

 
```
  //! Constructs a fuse of shapes aS1 and aS2.
  Standard_EXPORT BRepAlgoAPI_Fuse(const TopoDS_Shape& S1, const TopoDS_Shape& S2);
  
  //! Constructs a new shape that is a fuse of shapes aS1 and aS2 using aDSFiller.
  Standard_EXPORT BRepAlgoAPI_Fuse(const TopoDS_Shape& S1, const TopoDS_Shape& S2, const BOPAlgo_PaveFiller& aDSF);
```
 **使用实例：该代码是将两个基本矩形进行**布尔Union运算，将两个矩形合并生成新的形状，隐藏两个基本矩形的显示，只显示新生成的形状。

 
```
 //第一个基本矩形
gp_Pnt P(-5,5,-5);
TopoDS_Shape theBox1 = BRepPrimAPI_MakeBox(60, 200, 70).Shape();
Handle (AIS_Shape)	ais1 = new AIS_Shape(theBox1);
myAISContext->SetColor(ais1,Quantity_NOC_GREEN,Standard_False);
myAISContext->SetMaterial(ais1,Graphic3d_NOM_PLASTIC,Standard_False);
myAISContext->Display(ais1,Standard_False);
myAISContext->SetCurrentObject(ais1,Standard_False);
Fit();
Sleep(1000);

//第二个基本矩形
TopoDS_Shape theBox2 = BRepPrimAPI_MakeBox(P, 20, 150, 110).Shape();
Handle (AIS_Shape)	ais2 = new AIS_Shape(theBox2);
myAISContext->SetColor(ais2,Quantity_NOC_YELLOW,Standard_False);
myAISContext->SetMaterial(ais2,Graphic3d_NOM_PLASTIC,Standard_False);
myAISContext->Display(ais2,Standard_False);
myAISContext->SetCurrentObject(ais2,Standard_False);
Fit();
Sleep(1000);

//进行布尔Union运算，将两个图形合并
TopoDS_Shape FusedShape = BRepAlgoAPI_Fuse(theBox1,theBox2);

//隐藏两个基本矩形的显示
myAISContext->Erase(ais1,Standard_True);
myAISContext->Erase(ais2,Standard_True);

//显示经过布尔Union运算形成的新形状
Handle (AIS_Shape)	aFusion = new AIS_Shape(FusedShape);
myAISContext->SetDisplayMode(aFusion,1,Standard_False);
myAISContext->SetColor(aFusion,Quantity_NOC_RED,Standard_False);
myAISContext->SetMaterial(aFusion,Graphic3d_NOM_PLASTIC,Standard_False);
myAISContext->Display(aFusion,Standard_False);
myAISContext->SetCurrentObject(aFusion,Standard_False);
Fit();
```
 

   
 
