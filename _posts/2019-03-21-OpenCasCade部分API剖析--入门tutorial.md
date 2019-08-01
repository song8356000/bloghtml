---
layout: post
title: OpenCasCade部分API剖析--入门tutorial
date: 2019-03-21 11:14:53
---

{{ page.title }}
================

<p class="meta">2019-03-21 11:14:53 - 深圳</p>

   Tutorial简述

 
```
 gp_XXX
Standard_XXX
Geom_XXX
GC_MakeXXX
TopoDS_XXX
BRepBuilderAPI_XXX
BRepPrimAPI_XXX
BRepFilletAPI_XXX
TopExp_Explorer
TopAbs_ShapeEnum
BRepAlgoAPI_XXX
BRep_Tool与 Standard_Transient
TopTools_XXX
BRepOffsetAPI_XXX
```
 Tutorial源代码  
 OCC接触两年有余了，以前使用频率不高，好多东西一知半解，现在是需要用到它的时候了。

 **Tutorial简述**  
 Tutorial用一个绘制Bottle的例子描述了OCC建模的基本步骤。这里涉及了一些概念和类的用法，不细心看做笔记是很容易忘掉的。

 **gp_XXX**  
 gp_Pnt 是最简单的“点”；此外对应的有还有许多这样的简单结构,这些类从其类名就可以知道它可能有哪些参数，没错，这些类包含了点、线、面、轴以及各种曲面等，其中的参数都是构建这些几何结构必须的参数，我们可以称gp_Pnt这一类的类为基本几何类。   
 —————————————————————————————— 

 
```
 - gp_Ax2 
- gp_Ax2d 
- gp_Ax3 
- gp_Ax3d 
- gp_Ax22 
- gp_Ax22d 
- gp_Circ 
- gp_Circ2d 
- gp_Cone 
- gp_Cylinder 
- gp_Dir 
- gp_Dir2d 
- gp_Elips 
- gp_Elips2d 
- gp_EulerSequence 
- gp_GTrsf 
- gp_GTrsf2d 
- gp_Hypr 
- gp_Hypr2d 
- gp_Lin 
- gp_Lin2d 
- gp_Mat 
- gp_Mat2d 
- gp_Parab 
- gp_Parab2d 
- gp_Pln 
- gp_Pnt2d 
- gp_Quaternion 
- gp_QuaternionNLerp 
- gp_QuaternionSLerp 
- gp_Sphere 
- gp_Torus 
- gp_Trsf 
- gp_Trsf2d 
- gp_TrsfForm 
- gp_TrsfNLerp 
- gp_Vec 
- gp_Vec2d 
- gp_VectorWithNullMagnitude 
- gp_XY 
- gp_XYZ
```
 **Standard_XXX**  
 Standard_XXX类型的类是OCC的类型定义、宏定义的数据结构，与我们常用的C++没有区别的，我们可以称其为OCC定义数值类型。

 
```
 #define Standard_False false
#define Standard_True  true
typedef int           Standard_Integer;
typedef double        Standard_Real;
typedef bool          Standard_Boolean;
typedef float         Standard_ShortReal;
typedef char          Standard_Character;
typedef unsigned char Standard_Byte;
typedef void*         Standard_Address;
typedef size_t        Standard_Size;
typedef std::time_t   Standard_Time;

// Unicode primitives, char16_t, char32_t
typedef char          Standard_Utf8Char;     //!< signed   UTF-8 char
typedef unsigned char Standard_Utf8UChar;    //!< unsigned UTF-8 char
```
 **Geom_XXX**  
 Geom定义了几何数据结构，但也仅此而已，并不包含什么算法，可以说它是由gp_XXX构建而成的数据结构，比gp_XXX高级一些。我们可以称其为构建几何类。   
 ——————————————————————————————

 
```
 Geom_Axis1Placement 
Geom_Axis2Placement 
Geom_AxisPlacement 
Geom_BezierCurve 
Geom_BezierSurface 
Geom_BoundedCurve 
Geom_BoundedSurface 
Geom_BSplineCurve 
Geom_BSplineSurface 
Geom_CartesianPoint 
Geom_Circle 
Geom_Conic 
……
```
 **GC_MakeXXX**  
 GC_MakeXXX由简单的几何数据结构构建复杂的、常见的、带参数的几何结构Geom_XXX，它主要包含的就是构建算法，我们可以称其为几何形状构建包，构建的结果是指向Geom_XXX的指针。   
 —————————————————————————————— 

 
```
 GC_MakeArcOfCircle 
GC_MakeArcOfEllipse 
GC_MakeArcOfHyperbola 
GC_MakeArcOfParabola 
GC_MakeCircle 
GC_MakeConicalSurface 
GC_MakeCylindricalSurface 
GC_MakeEllipse 
GC_MakeHyperbola. 
GC_MakeLine 
GC_MakeMirror 
GC_MakePlane 
GC_MakeRotation 
GC_MakeScale 
GC_MakeSegment 
GC_MakeTranslation 
GC_MakeTrimmedCone 
GC_MakeTrimmedCylinder 
GC_Root

Handle(Geom_TrimmedCurve) aArcOfCircle = GC_MakeArcOfCircle(aPnt2,aPnt3,aPnt4);
```
 **TopoDS_XXX**  
 TopoDS_XXX比Geom_XXX再高一级，是多个Geom_XXX的一种组合。每一个TopoDS_XXX类都继承至TopoDS_Shape。我们可以称这一层为几何拓扑结构。包含了拓扑点、线、面、体等简单的、复杂的几何拓扑结构。注意：这层也只是数据结构，并不包含算法。   
 —————————————————————————————— 

 
```
 TopoDS_AlertWithShape 
TopoDS_Builder 
TopoDS_Compound 
TopoDS_CompSolid 
TopoDS_Edge 
TopoDS_Face 
TopoDS_FrozenShape 
TopoDS_HShape 
TopoDS_Iterator 
TopoDS_ListIteratorOfListOfShape 
TopoDS_ListOfShape 
TopoDS_LockedShape 
TopoDS_Shape 
TopoDS_Shell 
TopoDS_Solid 
TopoDS_TCompound 
TopoDS_TCompSolid 
TopoDS_TEdge 
TopoDS_TFace 
TopoDS_TShape 
TopoDS_TShell 
TopoDS_TSolid 
TopoDS_TVertex 
TopoDS_TWire 
TopoDS_UnCompatibleShapes 
TopoDS_Vertex 
TopoDS_Wire
```
 **BRepBuilderAPI_XXX**  
 前边说了TopoDS_XXX只是数据结构，那么如何由Geom_XXX构建TopoDS_XXX呢，这就是BRepBuilderAPI_XXX的工作了。我们可以称这一层为拓扑结构构建包。它的返回值直接就是TopoDS_XXX。

 此外这一部分有好多需要注意的事项:

 通过矩阵变换获取拓扑结构  
 通过向下转型(TopoDS::XXX)获取拓扑结构  
 通过TopoDS的组合获取TopoDS结构  
 也就是说：这一步骤相当于使用一个CAD、SolidWorks等的造型软件。熟悉造型的功能，想必更熟悉这部分要完成的工作。—————————————————————————————— 

 
```
 BRepBuilderAPI_BndBoxTreeSelector 
BRepBuilderAPI_CellFilter 
BRepBuilderAPI_Collect 
BRepBuilderAPI_Command 
BRepBuilderAPI_Copy 
BRepBuilderAPI_EdgeError 
BRepBuilderAPI_FaceError 
BRepBuilderAPI_FastSewing 
BRepBuilderAPI_FindPlane 
BRepBuilderAPI_GTransform 
BRepBuilderAPI_MakeEdge 
BRepBuilderAPI_MakeEdge2d 
BRepBuilderAPI_MakeFace 
BRepBuilderAPI_MakePolygon 
BRepBuilderAPI_MakeShape 
BRepBuilderAPI_MakeShell 
BRepBuilderAPI_MakeSolid 
BRepBuilderAPI_MakeVertex 
BRepBuilderAPI_MakeWire 
BRepBuilderAPI_ModifyShape 
BRepBuilderAPI_NurbsConvert 
BRepBuilderAPI_PipeError 
BRepBuilderAPI_Sewing 
BRepBuilderAPI_ShapeModification 
BRepBuilderAPI_ShellError 
BRepBuilderAPI_Transform 
BRepBuilderAPI_TransitionMode 
BRepBuilderAPI_VertexInspector 
BRepBuilderAPI_WireError

TopoDS_Edge aEdge1 = BRepBuilderAPI_MakeEdge(aSegment1); 
TopoDS_Edge aEdge2 = BRepBuilderAPI_MakeEdge(aArcOfCircle); 
TopoDS_Edge aEdge3 = BRepBuilderAPI_MakeEdge(aSegment2);
```
 **BRepPrimAPI_XXX**  
 这一层包的功能是把之前的拓扑结构建立成实体，它们构建的结果当然也是拓扑结果的TopoDS_XXX。我们可以称这一层为实体构建包，拓扑构建满足以下的规律：

 形状 生成形状  
 顶点(vertex) 边(edge)  
 边(edge) 面(face)  
 线(wire) 壳(shell)  
 面(face) 体(solid)  
 壳(shell) 组合体(compound of solids)  
 —————————————————————————————— 

 
```
 BRepPrimAPI_MakeBox 
BRepPrimAPI_MakeCone 
BRepPrimAPI_MakeCylinder 
BRepPrimAPI_MakeHalfSpace 
BRepPrimAPI_MakeOneAxis 
BRepPrimAPI_MakePrism 
BRepPrimAPI_MakeRevol 
BRepPrimAPI_MakeRevolution 
BRepPrimAPI_MakeSphere 
BRepPrimAPI_MakeSweep 
BRepPrimAPI_MakeTorus 
BRepPrimAPI_MakeWedge
```
 **BRepFilletAPI_XXX**  
 倒角包：计算倒角。   
 —————————————————————————————— 

 
```
 BRepFilletAPI_LocalOperation 
BRepFilletAPI_MakeChamfer 
BRepFilletAPI_MakeFillet 
BRepFilletAPI_MakeFillet2d
```
 **TopExp_Explorer**  
 这个类比较特殊，专门用于TopoDS_XXX的解析的，就是将已知实体(拓扑结构)解析为边组合、面组合等等。我们可以称其为拓扑解析包。

 
```
 for(TopExp_Explorer aFaceExplorer(myBody, TopAbs_FACE) ; aFaceExplorer.More() ; aFaceExplorer.Next())
{ 
    TopoDS_Face aFace = TopoDS::Face(aFaceExplorer.Current()); 
}

```
 **TopAbs_ShapeEnum**  
 这也是一个特殊的结构，类似一个拓扑结构的数组，具有.More().Next().Current()三个重要的方法，我们可以称其为拓扑解析结果集。

 **BRepAlgoAPI_XXX**  
 这个是核心算法包，专门用于Shape的交common(Boolean intersection)、差cut(Boolean subtraction)、和fuse(Boolean union)的计算。底层采用的是布尔操作。我们可以称其为几何算法包。   
 —————————————————————————————— 

 
```
 BRepAlgoAPI_Algo 
BRepAlgoAPI_BooleanOperation 
BRepAlgoAPI_BuilderAlgo 
BRepAlgoAPI_Check 
BRepAlgoAPI_Common 
BRepAlgoAPI_Cut 
BRepAlgoAPI_Defeaturing 
BRepAlgoAPI_Fuse 
BRepAlgoAPI_Section 
BRepAlgoAPI_Splitter
```
 **BRep_Tool与 Standard_Transient**  
 这个BRep_Tool类主要有三个方法，用于从TopoDS_XXX向Geom_XXX转换。 

 
```
 TopoDS_Face -> Geom_Surface 
TopoDS_Edge -> Geom_Curve 
TopoDS_Vertex -> Geom_Point 
```
 而**Standard_Transient**类主要有两个用途。   
**DynamicType** 函数用来获取 Handle(Geom_Surface)的真实类型，因为Geom_Surface有可能是任何一种面。   
 IsKind 用来判断该类型是否是某个类的子类。

 
```
 Handle(Geom_Surface) aSurface = BRep_Tool::Surface(aFace);

if(aSurface->DynamicType() == STANDARD_TYPE(Geom_Plane))
{ 
    Handle(Geom_Plane) aPlane = Handle(Geom_Plane)::DownCast(aSurface);
    //
}
```
 **TopTools_XXX**  
 组。List，Array，Sequence等。 

 
```
 TopTools_Array1OfListOfShape 
TopTools_Array1OfShape 
TopTools_Array2OfShape 
TopTools_DataMapIteratorOfDataMapOfIntegerListOfShape 
TopTools_DataMapIteratorOfDataMapOfIntegerShape 
……
```
 **BRepOffsetAPI_XXX**  
 非常有用的包，通俗点讲它的功能：如果你的线框模型可以生成实体，模型，那么它可以……里边是一个复杂而漫长的过程……我可以称他形体生成包

 
```
 BRepOffsetAPI_ThruSections aTool(Standard_True); 
aTool.AddWire(threadingWire1); 
aTool.AddWire(threadingWire2); 
aTool.CheckCompatibility(Standard_False); 
TopoDS_Shape myThreading = aTool.Shape();
```
 **Tutorial源代码**

 
```
 TopoDS_Shape MakeBottle(const Standard_Real myWidth, const Standard_Real myHeight, const Standard_Real myThickness) 
{ 
    // Profile : Define Support Points 
    gp_Pnt aPnt2(-myWidth / 2., -myThickness / 4., 0); 
    gp_Pnt aPnt3(0, -myThickness / 2., 0); 
    gp_Pnt aPnt4(myWidth / 2., -myThickness / 4., 0); 
    gp_Pnt aPnt5(myWidth / 2., 0, 0);
    // Profile : Define the Geometry 
    Handle(Geom_TrimmedCurve) aSegment1 = GC_MakeSegment(aPnt1, aPnt2); 
    Handle(Geom_TrimmedCurve) aSegment2 = GC_MakeSegment(aPnt4, aPnt5);
    // Profile : Define the Topology 
    TopoDS_Edge anEdge1 = BRepBuilderAPI_MakeEdge(aSegment1); 
    TopoDS_Edge anEdge3 = BRepBuilderAPI_MakeEdge(aSegment2); 
    TopoDS_Wire aWire = BRepBuilderAPI_MakeWire(anEdge1, anEdge2, anEdge3);
    // Complete Profile 
    gp_Ax1 xAxis = gp::OX(); 
    gp_Trsf aTrsf;
    aTrsf.SetMirror(xAxis); 
    BRepBuilderAPI_Transform aBRepTrsf(aWire, aTrsf); 
    TopoDS_Wire aMirroredWire = TopoDS::Wire(aMirroredShape);
    BRepBuilderAPI_MakeWire mkWire; 
    mkWire.Add(aMirroredWire); 
    TopoDS_Wire myWireProfile = mkWire.Wire();
    // Body : Prism the Profile 
    TopoDS_Face myFaceProfile = BRepBuilderAPI_MakeFace(myWireProfile); 
    gp_Vec aPrismVec(0, 0, myHeight); 
    TopoDS_Shape myBody = BRepPrimAPI_MakePrism(myFaceProfile, aPrismVec);
    // Body : Apply Fillets 
    BRepFilletAPI_MakeFillet mkFillet(myBody); 
    TopExp_Explorer anEdgeExplorer(myBody, TopAbs_EDGE); 
    while(anEdgeExplorer.More())
    { 
        TopoDS_Edge anEdge = TopoDS::Edge(anEdgeExplorer.Current()); //Add edge to fillet algorithm 
        mkFillet.Add(myThickness / 12., anEdge); 
        anEdgeExplorer.Next(); 
    }
    myBody = mkFillet.Shape();
    // Body : Add the Neck 
    gp_Pnt neckLocation(0, 0, myHeight); 
    gp_Dir neckAxis = gp::DZ(); 
    gp_Ax2 neckAx2(neckLocation, neckAxis);
    Standard_Real myNeckRadius = myThickness / 4.; 
    Standard_Real myNeckHeight = myHeight / 10.;
    BRepPrimAPI_MakeCylinder MKCylinder(neckAx2, myNeckRadius, myNeckHeight); 
    TopoDS_Shape myNeck = MKCylinder.Shape();
    myBody = BRepAlgoAPI_Fuse(myBody, myNeck);
    // Body : Create a Hollowed Solid 
    TopoDS_Face faceToRemove; 
    Standard_Real zMax = -1;
    for(TopExp_Explorer aFaceExplorer(myBody, TopAbs_FACE); aFaceExplorer.More(); aFaceExplorer.Next())
    { 
        TopoDS_Face aFace = TopoDS::Face(aFaceExplorer.Current()); // Check if <aFace> is the top face of the bottle’s neck 
        Handle(Geom_Surface) aSurface = BRep_Tool::Surface(aFace); 
        if(aSurface->DynamicType() == STANDARD_TYPE(Geom_Plane))
        { 
            Handle(Geom_Plane) aPlane = Handle(Geom_Plane)::DownCast(aSurface); 
            gp_Pnt aPnt = aPlane->Location(); 
            Standard_Real aZ = aPnt.Z(); 
            if(aZ > zMax)
            { 
                zMax = aZ; 
                faceToRemove = aFace; 
            }
        }
    }
    TopTools_ListOfShape facesToRemove; 
    facesToRemove.Append(faceToRemove); 
    BRepOffsetAPI_MakeThickSolid BodyMaker; 
    BodyMaker.MakeThickSolidByJoin(myBody, facesToRemove, -myThickness / 50, 1.e-3); 
    myBody = BodyMaker.Shape(); 
    // Threading : Create Surfaces 
    Handle(Geom_CylindricalSurface) aCyl1 = new Geom_CylindricalSurface(neckAx2, myNeckRadius * 0.99); 
    Handle(Geom_CylindricalSurface) aCyl2 = new Geom_CylindricalSurface(neckAx2, myNeckRadius * 1.05);
    // Threading : Define 2D Curves 
    gp_Pnt2d aPnt(2. * M_PI, myNeckHeight / 2.); 
    gp_Dir2d aDir(2. * M_PI, myNeckHeight / 4.); 
    gp_Ax2d anAx2d(aPnt, aDir);
    Standard_Real aMajor = 2. * M_PI; 
    Standard_Real aMinor = myNeckHeight / 10;
    Handle(Geom2d_Ellipse) anEllipse1 = new Geom2d_Ellipse(anAx2d, aMajor, aMinor); 
    Handle(Geom2d_Ellipse) anEllipse2 = new Geom2d_Ellipse(anAx2d, aMajor, aMinor / 4); 
    Handle(Geom2d_TrimmedCurve) anArc1 = new Geom2d_TrimmedCurve(anEllipse1, 0, M_PI); 
    Handle(Geom2d_TrimmedCurve) anArc2 = new Geom2d_TrimmedCurve(anEllipse2, 0, M_PI); 
    gp_Pnt2d anEllipsePnt1 = anEllipse1->Value(0); 
    gp_Pnt2d anEllipsePnt2 = anEllipse1->Value(M_PI);
    Handle(Geom2d_TrimmedCurve) aSegment = GCE2d_MakeSegment(anEllipsePnt1, anEllipsePnt2); 
    // Threading : Build Edges and Wires 
    TopoDS_Edge anEdge1OnSurf1 = BRepBuilderAPI_MakeEdge(anArc1, aCyl1); 
    TopoDS_Edge anEdge2OnSurf1 = BRepBuilderAPI_MakeEdge(aSegment, aCyl1); 
    TopoDS_Edge anEdge1OnSurf2 = BRepBuilderAPI_MakeEdge(anArc2, aCyl2); 
    TopoDS_Edge anEdge2OnSurf2 = BRepBuilderAPI_MakeEdge(aSegment, aCyl2); 
    TopoDS_Wire threadingWire1 = BRepBuilderAPI_MakeWire(anEdge1OnSurf1, anEdge2OnSurf1); 
    TopoDS_Wire threadingWire2 = BRepBuilderAPI_MakeWire(anEdge1OnSurf2, anEdge2OnSurf2); 
    BRepLib::BuildCurves3d(threadingWire1); 
    BRepLib::BuildCurves3d(threadingWire2);
    // Create Threading 
    BRepOffsetAPI_ThruSections aTool(Standard_True); 
    aTool.AddWire(threadingWire1); 
    aTool.AddWire(threadingWire2); 
    aTool.CheckCompatibility(Standard_False);
    TopoDS_Shape myThreading = aTool.Shape();
    // Building the Resulting Compound 
    TopoDS_Compound aRes; 
    BRep_Builder aBuilder; 
    aBuilder.MakeCompound (aRes); 
    aBuilder.Add (aRes, myBody); 
    aBuilder.Add (aRes, myThreading);
    return aRes;
}
```
 原文：https://blog.csdn.net/u012541187/article/details/81021064   
 

   
 
