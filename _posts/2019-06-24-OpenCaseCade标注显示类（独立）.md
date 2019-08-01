---
layout: post
title: OpenCaseCade标注显示类（独立）
date: 2019-06-24 16:58:50
---

{{ page.title }}
================

<p class="meta">2019-06-24 16:58:50 - 深圳</p>

   比如我在OCC绘图区画一个点P，然后在点P附近显示部分标注说明，如下图：

 ![](https://img-blog.csdnimg.cn/20190624165301402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9zdHVkeS1saWZlLmJsb2cuY3Nkbi5uZXQ=,size_16,color_FFFFFF,t_70)

 上图在指定位置 显示了指示性文字描述，OCC实现过程，一个类实现此功能 ISession_Text 类，在OCC例子程序中可以找到，这里是把它单独分离出来，只依赖OCC标准库文件。

 **ISession_Text.h:**

 
```
 // ISession_Text.h: interface for the ISession_Text class.
//
//////////////////////////////////////////////////////////////////////

#if !defined(AFX_ISESSION_TEXT_H__A9B277C4_A69E_11D1_8DA4_0800369C8A03__INCLUDED_)
#define AFX_ISESSION_TEXT_H__A9B277C4_A69E_11D1_8DA4_0800369C8A03__INCLUDED_

#if _MSC_VER >= 1000
#pragma once
#endif // _MSC_VER >= 1000

#include <Standard_Macro.hxx>
#include <Standard_DefineHandle.hxx>
#include <TCollection_AsciiString.hxx>
#include <Aspect_TypeOfText.hxx>
#include <Standard_Real.hxx>
#include <Standard_Integer.hxx>
#include <Quantity_Factor.hxx>
#include <Quantity_PlaneAngle.hxx>
#include <PrsMgr_PresentationManager3d.hxx>
#include <SelectMgr_Selection.hxx>
#include <Standard_OStream.hxx>
#include <Standard_IStream.hxx>
#include <Standard_CString.hxx>
#include <SelectMgr_SelectableObject.hxx>
//新增
#include "Prs3d_Text.hxx"
#include <AIS_Shape.hxx>
#include <V3d_Viewer.hxx>

class TCollection_AsciiString;
class SelectMgr_Selection;

DEFINE_STANDARD_HANDLE(ISession_Text,AIS_InteractiveObject)
class ISession_Text : public AIS_InteractiveObject  
{
public:
	ISession_Text();

    ISession_Text           (const TCollection_AsciiString& aText,
                             const Standard_Real            anX         = 0   ,
                             const Standard_Real            anY         = 0   ,
                             const Standard_Real            aZ          = 0   ,
                             const Aspect_TypeOfText        aType       = Aspect_TOT_SOLID,
                             const Quantity_PlaneAngle      anAngle     = 0.0 ,
                             const Standard_Real            aSlant      = 0.0 ,
                             const Standard_Integer         aColorIndex = 1   ,
                             const Standard_Integer         aFontIndex  = 1   ,
                             const Quantity_Factor          aScale      = 0.1   );
    ISession_Text
                            (const TCollection_AsciiString& aText, 
                             gp_Pnt&                        aPoint,
                             const Aspect_TypeOfText        aType       = Aspect_TOT_SOLID,
                             const Quantity_PlaneAngle      anAngle     = 0.0 ,
                             const Standard_Real            aSlant      = 0.0 ,
                             const Standard_Integer         aColorIndex = 1   ,
                             const Standard_Integer         aFontIndex  = 1   ,
                             const Quantity_Factor          aScale      = 0.1   );

	virtual ~ISession_Text();

inline   Standard_Integer        NbPossibleSelection() const;
inline   TCollection_AsciiString GetText() const;
inline   void                    SetText(const TCollection_AsciiString& atext) ;
inline   void                    GetCoord(Standard_Real& X, Standard_Real& Y, Standard_Real& Z) const ;
inline   void                    SetCoord(const Standard_Real X, const Standard_Real Y, const Standard_Real Z=0);
inline   Aspect_TypeOfText       GetTypeOfText() const;
inline   void                    SetTypeOfText(const Aspect_TypeOfText aNewTypeOfText) ;
inline   Standard_Real           GetAngle() const;
inline   void                    SetAngle(const Standard_Real aNewAngle) ;
inline   Standard_Real           GetSlant() const;
inline   void                    SetSlant(const Standard_Real aNewSlant) ;
inline   Standard_Integer        GetColorIndex() const;
inline   void                    SetColorIndex(const Standard_Integer aNewColorIndex) ;
inline   Standard_Integer        GetFontIndex() const;
inline   void                    SetFontIndex(const Standard_Integer aNewFontIndex) ;
inline   Quantity_Factor         GetScale() const;
inline   void                    SetScale  (const Quantity_Factor aNewScale) ;


DEFINE_STANDARD_RTTI(ISession_Text)

private: 

  void Compute          (const Handle(PrsMgr_PresentationManager3d)& aPresentationManager,
                        const Handle(Prs3d_Presentation)& aPresentation,
                        const Standard_Integer aMode);
  void Compute          (const Handle(Prs3d_Projector)& aProjector,
                        const Handle(Prs3d_Presentation)& aPresentation);
  void ComputeSelection (const Handle(SelectMgr_Selection)& aSelection,
                        const Standard_Integer unMode) ;


 // Fields PRIVATE
 //
TCollection_AsciiString MyText       ; 
Standard_Real           MyX          ;
Standard_Real           MyY          ;
Standard_Real           MyZ          ;
Aspect_TypeOfText       MyTypeOfText ;
Standard_Real           MyAngle      ;
Standard_Real           MySlant      ;
Standard_Integer        MyColorIndex ;
Standard_Integer        MyFontIndex  ;
Quantity_Factor         MyScale      ;
Standard_Real           MyWidth      ;
Standard_Real           MyHeight     ;


};

 inline Standard_Integer ISession_Text::NbPossibleSelection() const 
{ return 1; }

inline TCollection_AsciiString ISession_Text::GetText() const 
{  return MyText ; }

inline void ISession_Text::SetText(const TCollection_AsciiString& atext)
{  MyText = atext; }

inline void ISession_Text::GetCoord(Standard_Real& X, Standard_Real& Y, Standard_Real& Z) const 
{  X = MyX;  Y = MyY; Z = MyZ;}

inline void ISession_Text::SetCoord(const Standard_Real X, const Standard_Real Y, const Standard_Real Z)
{  MyX = X ;  MyY = Y ;  MyZ = Z ;}

inline Aspect_TypeOfText ISession_Text::GetTypeOfText() const 
{  return MyTypeOfText; }

inline void ISession_Text::SetTypeOfText(const Aspect_TypeOfText aNewTypeOfText)
{  MyTypeOfText = aNewTypeOfText; }

inline Standard_Real ISession_Text::GetAngle() const 
{  return MyAngle; }

inline void ISession_Text::SetAngle(const Standard_Real aNewAngle)
{  MyAngle = aNewAngle; }

inline Standard_Real ISession_Text::GetSlant() const 
{  return MySlant; }

inline void ISession_Text::SetSlant(const Standard_Real aNewSlant)
{  MySlant = aNewSlant; }

inline Standard_Integer ISession_Text::GetColorIndex() const 
{  return MyColorIndex; }

inline void ISession_Text::SetColorIndex(const Standard_Integer aNewColorIndex)
{  MyColorIndex = aNewColorIndex; }

inline Standard_Integer ISession_Text::GetFontIndex() const 
{ return MyFontIndex; }

inline void ISession_Text::SetFontIndex(const Standard_Integer aNewFontIndex)
{  MyFontIndex = aNewFontIndex; }

inline Quantity_Factor ISession_Text::GetScale() const 
{  return MyScale; }

inline void ISession_Text::SetScale(const Quantity_Factor aNewScale)
{  MyScale  = aNewScale; }

#endif // !defined(AFX_ISESSION_TEXT_H__A9B277C4_A69E_11D1_8DA4_0800369C8A03__INCLUDED_)

```
 **ISession_Text.cpp:**

 

 
```
 // ISession_Text.cpp: implementation of the ISession_Text class.
//
//////////////////////////////////////////////////////////////////////

#include "stdafx.h"
#include "ISession_Text.h"

#ifdef _DEBUG
#undef THIS_FILE
static char THIS_FILE[]=__FILE__;
//#define new DEBUG_NEW
#endif
IMPLEMENT_STANDARD_HANDLE(ISession_Text,AIS_InteractiveObject)
IMPLEMENT_STANDARD_RTTIEXT(ISession_Text,AIS_InteractiveObject)

//////////////////////////////////////////////////////////////////////
// Construction/Destruction
//////////////////////////////////////////////////////////////////////

ISession_Text::ISession_Text()
{

}

ISession_Text::ISession_Text
                 (const TCollection_AsciiString& aText, 
                  const Standard_Real            anX ,        // = 0
                  const Standard_Real            anY ,        // = 0
                  const Standard_Real            aZ  ,        // = 0
			      const Aspect_TypeOfText        aType,       // = SOLID,
			      const Quantity_PlaneAngle      anAngle,     // = 0.0
			      const Standard_Real            aslant,      // = 0.0
			      const Standard_Integer         aColorIndex, // = 0
			      const Standard_Integer         aFontIndex,  // = 1
			      const Quantity_Factor          aScale)      // = 1
                  :AIS_InteractiveObject(),MyText(aText),MyX(anX),MyY(anY),MyZ(aZ),
                  MyTypeOfText(aType),MyAngle(anAngle),MySlant(aslant),MyFontIndex(aFontIndex),
                  MyColorIndex(aColorIndex),MyScale(aScale),MyWidth(0),MyHeight(0)
{

}

ISession_Text::ISession_Text
                 (const TCollection_AsciiString& aText, 
                  gp_Pnt&                        aPoint,
			      const Aspect_TypeOfText        aType,       // = SOLID,
			      const Quantity_PlaneAngle      anAngle,     // = 0.0
			      const Standard_Real            aslant,      // = 0.0
			      const Standard_Integer         aColorIndex, // = 0
			      const Standard_Integer         aFontIndex,  // = 1
			      const Quantity_Factor          aScale)      // = 1
                  :AIS_InteractiveObject(),MyText(aText),MyX(aPoint.X()),MyY(aPoint.Y()),MyZ(aPoint.Z()),
                  MyTypeOfText(aType),MyAngle(anAngle),MySlant(aslant),MyFontIndex(aFontIndex),
                  MyColorIndex(aColorIndex),MyScale(aScale),MyWidth(0),MyHeight(0)
{

}

ISession_Text::~ISession_Text()
{

}

void ISession_Text::Compute(const Handle(PrsMgr_PresentationManager3d)& /*aPresentationManager*/,
                            const Handle(Prs3d_Presentation)& aPresentation,
                            const Standard_Integer /*aMode*/)
{
    Prs3d_Text::Draw(aPresentation,myDrawer,MyText,gp_Pnt(  MyX ,MyY,MyZ ));
}

void ISession_Text::Compute(const Handle(Prs3d_Projector)& /*aProjector*/,
                            const Handle(Prs3d_Presentation)& /*aPresentation*/) 
 {
 }

void ISession_Text::ComputeSelection(const Handle(SelectMgr_Selection)& /*aSelection*/, 
				     const Standard_Integer /*unMode*/)
{
}


```
 

 **简单使用例子：**

 
```
 void OccEditView::DrawPoint(const Standard_Real Xp, const Standard_Real Yp, const Standard_Real Zp)
{
	gp_Pnt P(Xp,Yp,Zp);
	Handle(AIS_Point) aPoint = new AIS_Point(new Geom_CartesianPoint(P)); 
	myAISContext->SetColor(aPoint,Quantity_NOC_BLUE3);
	myAISContext->Display(aPoint); 


    //在实例化类的时候输入需要显示的字符，目前不支持中文
	Handle(ISession_Text)  aGraphicText = new ISession_Text("This a Point!",P.X()+50.,P.Y()+10.,P.Z()+10.,);
	myAISContext->Display(aGraphicText); 
}
```
 

   
 
