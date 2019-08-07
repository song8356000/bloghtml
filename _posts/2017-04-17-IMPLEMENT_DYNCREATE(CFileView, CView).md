---
title: IMPLEMENT_DYNCREATE(CFileView, CView)
date: 2018-08-23 11:46:39
tags: CSDN迁移
---
   CFileView 是你当前生成的类名 CView是你当前生成的类的父类 类库用IMPLEMENT_DYNCREATE宏来动态创建对象，例如可以实现读取并创建一个存储在磁盘上的对象。

 使用方法：将IMPLEMENT_DYNCREATE宏加入到你的类实现文件中即可。 如果你一起始用了DECLARE_DYNCREATE和IMPLEMENT_DYNCREATE宏，你就可以使用RUNTIME_CLASS宏和CObject类的IsKindOf成员函数以在运行时确定你的对象是由哪个类生成的。 类声明中包含DECLARE_DYNCREATE，那么类的实现文件中必须包含IMPLEMENT_DYNCREATE  
 

 

 IMPLEMENT_DYNCREATE(class_name,base_class_name)  
 说明：  
 通过DECLARE_DYNCREATE宏来使用IMPLEMENT_DYNCREATE宏，以允许CObject派生类对象在运行时自动建立。主机使用此功能自动建立对象，例如，但它在串行化过程中从磁盘读去一个对象时，他在类工具里加入IMPLEMENT_DYNCREATE宏。若用户使用DECLARE_DYNCREATE和IMPLEMENT_DYNCREATE宏,那么接着使用RUNTIME_CLASS宏和CObject::IsKindOf成员函数以在运行时确定对象类。若declare_dyncreate包含在定义中，那么IMPLEMENT_DYNCREATE必须包含在类工具中

   
 