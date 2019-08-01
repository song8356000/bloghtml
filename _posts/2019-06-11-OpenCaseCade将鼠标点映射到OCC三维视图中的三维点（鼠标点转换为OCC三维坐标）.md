---
layout: post
title: OpenCaseCade将鼠标点映射到OCC三维视图中的三维点（鼠标点转换为OCC三维坐标）
date: 2019-06-11 10:54:58
---

{{ page.title }}
================

<p class="meta">2019-06-11 10:54:58 - 深圳</p>

 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/91415075   
   有段时间没写过关于OCC的文章，前段时间在以前写过的[一篇文章 ](https://blog.csdn.net/qq_22642239/article/details/89306039)介绍将鼠标点转换为OCC三维坐标，在一位博友的提示中，以另外一种方式实现，这种方法就是先构造一条过鼠标点的并且垂直于屏幕的一条线，然后求该直线与某一个面的交点，得到该交点的三维坐标，即是该鼠标点转换为OCC的三为坐标。

 代码实现：

 
```
 //将鼠标位置坐标转换为OCC三维坐标（垂直线法）
gp_Pnt OccEditView::ChangeCoordinateSecond(double x,double y)
{
	gp_Pnt P0,P1,P3;
	gp_Vec Vp2;
	Handle(Geom_Curve) aCurve;
	double X,Y,Z,VX,VY,VZ;
	myView->Convert(int(x),int(y),X,Y,Z);
	myView->Proj(VX,VY,VZ);
	P1.SetCoord(X,Y,Z);
	Vp2.SetCoord(VX,VY,VZ);
	gp_Lin gpLin(P1,gp_Dir(Vp2));
	
	aCurve=GC_MakeSegment(gpLin,-10000,10000);
	
	GeomAPI_IntCS CS (aCurve,aGeometricSurface);   //aGeometricSurface为与直线相交的面

	Standard_Integer NbSeg = 0;
	Standard_Integer NbPoints = 0;
	Handle(Geom_Curve) segment;
	if(CS.IsDone())
	{
		NbSeg = CS.NbSegments();
		for (Standard_Integer k=1; k<=NbSeg; k++)
		{
			segment = CS.Segment(k);
		}

		NbPoints = CS.NbPoints();
		for (int k=1; k<=NbPoints; k++)
		{
			gp_Pnt aPoint=CS.Point(k);
			P3=aPoint;
		}
	}
	return P3;
}

```
 上述代码中aGeometricSurface 为与直线相交的面，该面可以在鼠标选中某个面时求得，在计算一个面的法向量的时候得出，也可以自己通过其他途径得到。

 与平面的垂直线最好设置的长一点，这样更容易求得交点，有时会出现多个交点，这个自己根据相交曲面的实际情况考虑。此处默认只有一个交点。

   
 
