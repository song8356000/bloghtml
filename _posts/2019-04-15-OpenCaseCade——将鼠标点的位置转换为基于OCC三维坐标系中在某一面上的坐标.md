---
layout: post
title: OpenCaseCade——将鼠标点的位置转换为基于OCC三维坐标系中在某一面上的坐标
date: 2019-04-15 10:15:28
---

{{ page.title }}
================

<p class="meta">2019-04-15 10:15:28 - 深圳</p>

 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/89306039   
   在使用OCC过程中遇见很多难点，而且这方面的资料也比较少，有时候看似很简单的问题如果找不到正确的切入点，很难快速找到解决方法。

 在使用OCC过程中，坐标转换是必须要用到的，下面分析一下我在将鼠标位置（x，y）二维点坐标转换为OCC三维坐标系中，落在某一选中面上的简单方法（目前所做到的是将选中面切换至当前视图，也就是将选中面切换至正对着屏幕，如果不切换至正对屏幕，则转换的坐标有小的偏差，这点还不是很清楚如何处理）。

 下面是我用到的三个函数，功能就是做鼠标到选中面的坐标转换，代码如下：

 1.首先需要将选中面转换为gp_Pln平面

 输入参数是TopoDS_Face类型的面，返回的是面的法向量，agpPlane为gp_Pln面。

 
```
 //求一个面的法向量
gp_Vec CMFCOCCView::NormalVector(TopoDS_Face F)   
{
	TopLoc_Location location;
	Handle (Geom_Surface) aGeometricSurface = BRep_Tool::Surface(F,location);
	Handle (Geom_Plane) aPlane = Handle (Geom_Plane)::DownCast(aGeometricSurface);
	agpPlane = aPlane->Pln();       //将选中TopoDS_Face面转换为gp_Pln平面，agpPlane为全局变量
	gp_Ax1 norm = agpPlane.Axis();
	gp_Dir dir = norm.Direction();
	gp_Vec move=gp_Vec(dir);
	return move;
}
```
 

 2.然后在gp_Pln平面的基础上进行转换

 输入参数第一个与第二个是鼠标点point（x，y）的x与y值，第三个参数是V3d_View类型的（V3d_View类），也就是应用程序对象VIEW

 
```
 //将鼠标二维坐标点转换为OCC三维坐标系中选中面上的三维坐标点
gp_Pnt ConvertClickToPoint(Standard_Real x, Standard_Real y, Handle(V3d_View) aView)
{ 

    gp_Pln PlaneOfTheView = agpPlane;  //注意agpPlane是全局变量，创建一个平面，
                                       //用于将鼠标点投射在此面上，这是将鼠标点投射在选定面上的
                                       //基础
    Standard_Real X,Y,Z;
    aView->Convert(int(x),int(y),X,Y,Z);  //将鼠标点坐标转换为OCC三维坐标
    gp_Pnt ConvertedPoint(X,Y,Z);
    gp_Pnt2d ConvertedPointOnPlane = ProjLib::Project(PlaneOfTheView,ConvertedPoint);

    gp_Pnt ResultPoint = ElSLib::Value(ConvertedPointOnPlane.X(),
        ConvertedPointOnPlane.Y(),
        PlaneOfTheView);

    return ResultPoint;     //将鼠标点二维坐标转换为基于OCC三维坐标选定面上的坐标（OCC三维坐标）
}
```
 使用此功能我实现的是在鼠标移动时在选中面上画指定图元（线段，圆等），这只是坐标转换部分关键功能，其他可以根据自己的使用需求添加，这种方式还有一点没有实现的功能就是：如何计算在选中面不是正对屏幕时，如何定义一种规则让鼠标移动的点以定义的规则的方式投影到选中面上，现在虽然可以实现，但是选中面不是正对屏幕的时候，坐标转换 不是很完美，需要进一步了解，有更好的方式的话，交流一下。

   
 
