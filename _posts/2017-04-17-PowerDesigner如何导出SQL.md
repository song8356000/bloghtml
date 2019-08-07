---
title: PowerDesigner如何导出SQL
date: 2017-12-25 17:42:47
tags: CSDN迁移
---
   PowerDesigner  
  PowerDesigner最初由Xiao-Yun Wang（[王晓昀](http://baike.baidu.com/view/1258504.htm)）在SDP Technologies公司开发完成。  
 PowerDesigner是[Sybase](http://baike.baidu.com/view/118488.htm)的企业建模和设计解决方案，采用模型驱动方法，将业务与IT结合起来，可帮助部署有效的企业体系架构，并为研发生命周期管理提供强大的分析与设计技术。  
  powerDesigner pdm转换成sql  
   
 一、选择菜单栏上Database  
 ![img](http://shp.qpic.cn/collector/786132874/305d8277-08df-4c3b-b2bf-18ecf9c0c3e7/0)  
  二、更改当前数据库类型>>Change Current DBMS 按钮  
 ![img](http://shp.qpic.cn/collector/786132874/54408ec3-1a39-4607-ab52-41ab6de92a84/0)  
   三、将当前的DBMS更改成需要的DBMS（如果用的就是当前的mysql5.0就无需更换）  
 ![img](http://shp.qpic.cn/collector/786132874/234cf13f-3ad4-4fd7-a6f9-08be8ea4d9c8/0)  
 四、选择Database菜单栏下 >>Generate Database  
 ![img](http://shp.qpic.cn/collector/786132874/344eed2c-088a-4be8-ad2d-d81f89748be8/0)  
  五、设置导出.sql名字和路劲 ，配置如图所示。  
 ![img](http://shp.qpic.cn/collector/786132874/9bdacfc5-a5cc-4310-9d55-7d9b66a364c5/0)  
  六、导出的sql是没有注释的，如果需要注释  
  Tools >>Excute Commands >> Edit/Run Script打开的窗口中添加以下信息  
  '******************************************************************************   
 '* File: name2comment.vbs   
 '* Purpose: Database generation cannot use object names anymore   
 ' in version 7 and above.   
 ' It always uses the object codes.   
 '  
 ' In case the object codes are not aligned with your   
 ' object names in your model, this script will copy   
 ' the object Name onto the object Comment for   
 ' the Tables and Columns.   
 '  
 '* Title:   
 '* Version: 1.0   
 '* Company: Sybase Inc.   
 '******************************************************************************

 Option Explicit  
 ValidationMode = True  
 InteractiveMode = im_Batch

 Dim mdl ' the current model

 ' get the current active model   
 Set mdl = ActiveModel   
 If (mdl Is Nothing) Then  
 MsgBox "There is no current Model "  
 ElseIf Not mdl.IsKindOf(PdPDM.cls_Model) Then  
 MsgBox "The current model is not an Physical Data model. "  
 Else  
 ProcessFolder mdl   
 End If

 ' This routine copy name into comment for each table, each column and each view   
 ' of the current folder   
 Private sub ProcessFolder(folder)   
 Dim Tab 'running table   
 for each Tab in folder.tables   
 if not tab.isShortcut then  
 '把表明作为表注释，其实不用这么做  
 tab.comment = tab.name   
 Dim col ' running column   
 for each col in tab.columns   
 '把列name和comment合并为comment  
 col.comment= col.name   
 next  
 end if  
 next

 Dim view 'running view   
 for each view in folder.Views   
 if not view.isShortcut then  
 view.comment = view.name   
 end if  
 next

 ' go into the sub-packages   
 Dim f ' running folder   
 For Each f In folder.Packages   
 if not f.IsShortcut then  
 ProcessFolder f   
 end if  
 Next  
 end sub

 

 执行。再执行步骤一到步骤五

    
   
 