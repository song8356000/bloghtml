---
layout: post
title: OpenCaseCade网格的显示
date: 2019-05-08 14:31:55
---

{{ page.title }}
================

<p class="meta">2019-05-08 14:31:55 - 深圳</p>

 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/89951487   
   首先看一张图：

 ![](https://img-blog.csdnimg.cn/20190508142444701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9zdHVkeS1saWZlLmJsb2cuY3Nkbi5uZXQ=,size_16,color_FFFFFF,t_70)

 上图中间的网格区域，是OCC提供的，可以给我们提供参考作用，那么这个网格如何显示出来，并设置属性呢，其实在

 V3d_Viewer这个类中有提供此类方法：

 下面两种方法，实现了网格的显示与隐藏，其他具体参数可以看OCC代码结构，有详细介绍。

 
```
  //! Activates the grid in all views of <me>.
  Standard_EXPORT   void ActivateGrid (const Aspect_GridType aGridType, const Aspect_GridDrawMode aGridDrawMode) ;
  
  //! Deactivates the grid in all views of <me>.
  Standard_EXPORT   void DeactivateGrid() ;
```
 

 使用方法，在OCC环境初始化的时候激活网格

 
```
 myViewer->ActivateGrid (Aspect_GT_Rectangular, Aspect_GDM_Lines) ;
```
 

   
 
