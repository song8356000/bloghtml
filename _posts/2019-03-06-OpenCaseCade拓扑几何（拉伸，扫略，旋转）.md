---
layout: post
title: OpenCaseCade拓扑几何（拉伸，扫略，旋转）
date: 2019-03-06 18:47:22
---

{{ page.title }}
================

<p class="meta">2019-03-06 18:47:22 - 深圳</p>

   OCC提供几种图形的构建是由基本图形的旋转，拉伸等方式形成的，源码例子有相关的介绍。

 下面介绍的一些实例提供思路与核心实现代码，显示与属性需要自己添加。

 关于OCC集合拓扑结构如下：

 现在经常接触的就是BRepPrimAPI_MakeSweep ，使用到的就是其三个派生类

 

 ![](https://img-blog.csdnimg.cn/20190306183113879.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyNjQyMjM5,size_16,color_FFFFFF,t_70)

 

 

 
### **BRepPrimAPI_MakePrim**

 ****(1) ****** **功能说明：swept(拉伸)

 ****(2) ****** **构造函数

 
```
 BRepPrimAPI_MakePrism(TopoDS_Shape S, gp_Vec V, bool Copy, bool Canonize);

BRepPrimAPI_MakePrism(TopoDS_Shape S, gp_Dir D, bool Inf, bool Copy, bool Canonize);
```
 ****(3) ****** **参数说明

 TopoDS_Shape：扫掠拓扑对象

 gp_Vec：扫掠的方向及大小（向量）

 gp_Dir：扫掠的方向及大小（单位向量）

 Copy：默认值为FALSE; 若为TRUE，则得用扫掠对象副本进行扫掠

 Canonize：默认值为TRUE;

 Inf:默认值为TRUE; if Inf is false the prism is semi-infinite (in the direction D).

 ****(4) ****** 实例**

 **例1：拓扑点扫掠成边**

 
```
 TopoDS_Vertex V1 =new BRepBuilderAPI_MakeVertex(new gp_Pnt(-200, -200, 0)).Vertex();

TopoDS_Shape S1 =new BRepPrimAPI_MakePrism(V1, new gp_Vec(0, 0, 100), false, false).Shape();


```
 **例2：拓扑边扫掠成面**

 
```
 TopoDS_Edge E =new BRepBuilderAPI_MakeEdge(new gp_Pnt(-150, -150, 0),new gp_Pnt(-50, -50, 0)).Edge();

TopoDS_Shape S2 =new BRepPrimAPI_MakePrism(E, new gp_Vec(0, 0, 100), false, false).Shape();
```
 **例3：拓扑边框扫掠成体**

 
```
 TopoDS_Edge E1，E2，E3；

TopoDS_Wire W = new BRepBuilderAPI_MakeWire(E1, E2, E3).Wire();

TopoDS_Shape S3 =new BRepPrimAPI_MakePrism(W, new gp_Vec(0, 0, 100), false, false).Shape();


```
 ![](https://img-blog.csdnimg.cn/20190306183752519.png)

 

 
### **BRepPrimAPI_MakeRevol**

 ****(1) ****** **功能说明：旋转面

 ****(2) ****** **构造函数

 
```
 public BRepPrimAPI_MakeRevol(TopoDS_Shape S, gp_Ax1 A, bool Copy);

public BRepPrimAPI_MakeRevol(TopoDS_Shape S, gp_Ax1 A, double D, bool Copy);
```
 ****(3) ****** **参数说明

 TopoDS_Shape：要进行旋转的拓扑对象

 gp_Ax1：用某一坐标方向的直线作为旋转轴

 Copy：是否用要旋转对象的副本来进行操作，默认值为FALSE;

 D：旋转的角度；

 

 ****(4) ****** **实例

 例1：拓扑点形成圆弧

 
```
 TopoDS_Vertex V1;

gp_Ax1 axe = new gp_Ax1(new gp_Pnt(-170, -170, 0), new gp_Dir(0, 0, 1));

Geom_Axis1Placement Gax1 = new Geom_Axis1Placement(axe);

AIS_Axis ax1 = new AIS_Axis(Gax1);

TopoDS_Shape S1 = new BRepPrimAPI_MakeRevol(V1, axe, false).Shape();
```
 例2：拓扑边形成柱面

 
```
 TopoDS_Edge E;

AIS_Shape ais3 = new AIS_Shape(E);

gp_Ax1 axe = new gp_Ax1(new gp_Pnt(-100, -100, 0), new gp_Dir(0, 0, 1));

Geom_Axis1Placement Gax2 = new Geom_Axis1Placement(axe);

AIS_Axis ax2 = new AIS_Axis(Gax2);

TopoDS_Shape S2 = new BRepPrimAPI_MakeRevol(E, axe, false).Shape();
```
 例3：拓扑线框形成环体

 
```
 TopoDS_Edge E1， E2， E3;

TopoDS_Wire W = new BRepBuilderAPI_MakeWire(E1, E2, E3).Wire();

gp_Ax1 axe = new gp_Ax1(new gp_Pnt(0, 0, 30), new gp_Dir(0, 1, 0));

Geom_Axis1Placement Gax3 = new Geom_Axis1Placement(axe);

AIS_Axis ax3 = new AIS_Axis(Gax3);

TopoDS_Shape S3 =new BRepPrimAPI_MakeRevol(W, axe, 210 * PI / 180, false).Shape();
```
 ![](https://img-blog.csdnimg.cn/20190306184214294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyNjQyMjM5,size_16,color_FFFFFF,t_70)

 

 

 
### **BRepOffsetAPI_MakePipe**

 ****(1) ****** **功能说明：pipe管道

 该类建立一个基础Shape（call profile）沿一个线框（call spine--脊骨）扫掠。

 Profile不能包含solids;

 该类提供如下功能处理：

 定义管道的构造，实现构造算法且可查看结果。

 注意：该类实现管道的构造仅针对G1连续的spine。

 如果是线框应用管道后，变成曲面；如果是面应用管道后，变成体；

 ****(2) ****** **构造函数

 
```
 public BRepOffsetAPI_MakePipe(TopoDS_Wire Spine, TopoDS_Shape Profile);
```
 ****(3) ****** **参数说明

 Spine:管道的轨迹线；

 Profile：沿轨迹线进行扫掠的拓扑SHAPE；

 ****(4) ****** **实例

 例1：一个圆面，沿一个曲线（线框）进行扫掠，形成一个SOLID

 
```
 TopoDS_Wire W ;          

gp_Circ c =new gp_Circ(new gp_Ax2(new gp_Pnt(0, 0, 0), new gp_Dir(0, 1, 0)), 10);

TopoDS_Edge Ec = new BRepBuilderAPI_MakeEdge(c).Edge();

TopoDS_Wire Wc = new BRepBuilderAPI_MakeWire(Ec).Wire();          

TopoDS_Face F = new BRepBuilderAPI_MakeFace(Wc, true).Face();

TopoDS_Shape S = new BRepOffsetAPI_MakePipe(W, F).Shape();


```
 ![](https://img-blog.csdnimg.cn/2019030618433048.png)

 

 

 

 
### **BRepOffsetAPI_ThruSections**

 ****(1) ****** **功能说明：

 Initializes an algorithm for building a shell or a solid passing through a set of sections；

 ****(2) ****** **构造函数

 
```
 public BRepOffsetAPI_ThruSections(bool isSolid, bool ruled, double pres3d)
```
 ****(3) ****** **参数说明

 isSolid：默认值为FALSE;若为真，则表示算法需要建立一个SOLID，否则，表示，需要建立一个SHELL；

 ruled：默认值为FALSE; 是否规则

 ruled=true： if the faces generated betweenthe edges of two consecutive wires are ruled surfaces;

 ruled=false： if they are smoothed out by approximation

 pres3d：defines the precision criterion used by the approximation algorithm。默认值为：1.0e-6

 ****(4) ****** **实例

 例1：通过四个SECTION形成一个SOLID；

 
```
 gp_Circ c1b =new gp_Circ(new gp_Ax2(new gp_Pnt(100, 0, -100), new gp_Dir(0, 0, 1)), 40);

TopoDS_Edge E1b = new BRepBuilderAPI_MakeEdge(c1b).Edge();

TopoDS_Wire W1b = new BRepBuilderAPI_MakeWire(E1b).Wire();

gp_Circ c2b =new gp_Circ(new gp_Ax2(new gp_Pnt(210, 0, -0), new gp_Dir(0, 0, 1)), 40);

TopoDS_Edge E2b = new BRepBuilderAPI_MakeEdge(c2b).Edge();

TopoDS_Wire W2b = new BRepBuilderAPI_MakeWire(E2b).Wire();
       
gp_Circ c3b =new gp_Circ(new gp_Ax2(new gp_Pnt(275, 0, 100), new gp_Dir(0, 0, 1)), 40);

TopoDS_Edge E3b = new BRepBuilderAPI_MakeEdge(c3b).Edge();

TopoDS_Wire W3b = new BRepBuilderAPI_MakeWire(E3b).Wire();

gp_Circ c4b =new gp_Circ(new gp_Ax2(new gp_Pnt(200, 0, 200), new gp_Dir(0, 0, 1)), 40);

TopoDS_Edge E4b = new BRepBuilderAPI_MakeEdge(c4b).Edge();

TopoDS_Wire W4b = new BRepBuilderAPI_MakeWire(E4b).Wire();

BRepOffsetAPI_ThruSections generatorb =new BRepOffsetAPI_ThruSections(true, false, 1.0e-06);

generatorb.AddWire(W1b);

generatorb.AddWire(W2b);

generatorb.AddWire(W3b);

generatorb.AddWire(W4b);

generatorb.Build();

TopoDS_Shape S2 = generatorb.Shape();
```
 ![](https://img-blog.csdnimg.cn/2019030618460321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyNjQyMjM5,size_16,color_FFFFFF,t_70)

 

 
### ** BRepOffsetAPI_MakePipeShell**

 说明：

 该类提供了一个框架，沿着脊柱的导线构建一个shell或者solid。

 构造solid，初始线框必须闭合的。

 主要通过两种方法进行定义：

 一、截面

 二、定义扫掠方式

 

 备注：本文参考自一个OCC文档原作者不详。

   
 
