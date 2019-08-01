---
layout: post
title: OpenCascade一些常用的API
date: 2019-03-21 11:07:43
---

{{ page.title }}
================

<p class="meta">2019-03-21 11:07:43 - 深圳</p>

   一、几何图形部分

 **三维点gp_point**  
 gp_point(0., 0., 0.) 三维坐标构建一个点

 gp_point::X(); gp_point::Y(); gp_point::Z(); 可以取得已知点的X、Y、Z坐标

 gp_point::SetX(); gp_point::SetY(); gp_point::SetZ(); gp_point::SetCoord(); 可以设置三维点的坐标

 2**.边TopoDS_Edge**

 BRepBuilderAPI_MakeEdge创建边

 BRepBuilderAPI_MakeEdge(const gp_Pnt& P1, const gp_Pnt& P2); 通过两个点构造边

 BRepBuilderAPI_MakeEdge(const gp_Lin& L); 通过射线构造边

 此外gp_Circ（圆）、gp_Elips（椭圆）、gp_Hypr（双曲线一支）、gp_Parab（抛物线）、Geom_Curve（弧线）、Geom2d_Curve（二维弧线）等亦可构建边

 

 3.**线网格TopoDS_Wire**

 BRepBuilderAPI_MakeWire创建线

 BRepBuilderAPI_MakeWire::Add 添加线

 BRepBuilderAPI_MakeWire::IsDone 判断添加边是否有效

 BRepBuilderAPI_MakeWire::Error 返还构建结果状态

 BRepBuilderAPI_MakeWire::Wire 返还构建的网格

 BRepBuilderAPI_MakeWire::Edge 返还构建网格的最后一边（与原始边可能不同）

 BRepBuilderAPI_MakeWire::Vertex 返还构建网格的最后一边的顶点？

 目前已知，添加多条边时，若边之间不相交，会出现不可预知的错误。

 4.**面TopoDS_Face**

 BRepBuilderAPI_MakeFace创建面

 可通过gp_Pln、gp_Cylinder、gp_Cone、gp_Sphere、gp_Torus等构造面

 BRepBuilderAPI_MakeFace::Add 添加线

 BRepBuilderAPI_MakeFace::IsDone 构成一个有效面则返还true

 BRepBuilderAPI_MakeFace::Error 返还构建结果状态

 BRepBuilderAPI_MakeFace::Face 返回构建的面

 5.**体TopoDS_Shape**

 5.1 **gp_Circ 创建圆**

 gp_Circ::gp_Circ(const gp_Ax2& A2, const Standard_Real Radius);中心轴和半径构建一个圆。

 

 5.2 BRepPrimAPI_MakeBox可创建矩形体

 5.3 BRepPrimAPI_MakeWedge创建楔形体（楔形体就是带斜面的长方体，即带角度的长方体。）

 5.4 BRepPrimAPI_MakeOneAxis创建旋转体（基类）

 5.4.1 BRepPrimAPI_MakeCylinder创建圆柱体

 5.4.2 BRepPrimAPI_MakeCone创建圆锥体

 5.4.3 BRepPrimAPI_MakeSphere创建球体

 5.4.4 BRepPrimAPI_MakeTorus创建圆环体

 5.4.5 BRepPrimAPI_MakeRevolution创建旋转体

 5.5 BRepPrimAPI_MakeSweep创建扫掠体（基类）

 5.5.1 BRepOffsetAPI_MakePipe 创建管道

 5.5.2 BRepOffsetAPI_MakePipeShell

 5.5.3 BRepPrimAPI_MakePrism创建拉伸体

 5.5.4 BRepPrimAPI_MakeRevol创建旋转体

 5.6 TopoDS_Compound 复合体

 BRep_Builder builder;

 TopoDS_Compound Comp;

 TopoDS_Shape S1, S2;

 builder.Add(Comp, S1);

 builder.Add(Comp, S2);

 

 6. **gp_Trsf 几何变换**

 gp_Trsf::SetMirror 镜像变换

 gp_Trsf::SetRotation 角度旋转变换

 gp_Trsf::SetScale 缩放变换

 gp_Trsf::SetTranslation 平移变换？

 7. BRepAlgoAPI_BooleanOperation图形布尔运算

 7.1 BRepAlgoAPI_Fuse布尔并运算

 7.2 BRepAlgoAPI_Common布尔交运算

 7.3 BRepAlgoAPI_Cut布尔差运算

 7.4 BRepAlgoAPI_Section 剖面

 

 8. 类型转换类  
 目前用到的两个类，处理一些不同类型之间的转换：

 8.1 TopoDS

 TopoDS_Shape 向相应的格式转

 例如：Vertex = TopoDS::Vertex(Shape);

 8.2 BRep_Tool

 TopoDS_Vertex向gp_Pnt转

 gp_Pnt P = BRep_Tool::Pnt(Vertex)

 

   
 
